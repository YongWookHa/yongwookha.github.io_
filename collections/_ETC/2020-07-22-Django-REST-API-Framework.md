---
layout: post
title: 이미지 전송 REST-API 서버 구축
subtitle: feat. django-rest-framework
tags: [DEVELOP]
image: /img/django.png
comments: true
---

그동안 매일같이 바쁜 나날들을 보내오면서 꽤 많은 경험을 했다. 요즘에는 R&D에만 시간을 쏟지 않고, AI 코어와 Front Serving까지 아우를 수 있는 풀스택 AI 개발자의 포지션을 잡아보려고 이것저것 열심히 공부하고 있다. 올 상반기에 회사에서 연구, 개발했던 OCR 엔진을 웹에서 서비스하기 위해 Django 기반의 간단한 REST-API 서버를 만들었다.  

OCR 엔진은 텍스트 검출과 인식으로 구분되는 전형적인 OCR 방식으로 설계되었고, 서비스는 지정된 IP와 Port로 지급받은 API-Key와 함께 이미지를 전송하면 이를 처리하여 텍스트 결과로 보내주는 방식으로 이루어진다. 

## 이 포스트에서 다룰 내용  
이 포스트에서는 Django에서 기본으로 제공하는 User 템플릿을 수정하여 각 User가 랜덤한 API-Key를 지급받도록 하고, Django-Rest-Framework 패키지를 이용하여 권한이 있는 유저가 서비스에 접근할 수 있도록 하는 Customized 코드에 대해 소개하고, 그 일부를 발췌하여 설명한다. 

이 코드를 이용하여 유저는 다음 두가지 방법으로 서비스에 접근할 수 있다.

**1. 단일 이미지에 대한 결과 얻기 (수동)**  
html form으로 웹 브라우저에서 이미지 업로드 및 API-key를 입력하고 request하여 response 얻기  
**2. 복수 이미지에 대한 결과 얻기 (자동)**  
base64 encoded image와 API-Key를 json format으로 request하고 response 얻기  

전체 코드는 Github Repo에서 확인할 수 있다. 전체 코드를 보면서 아래 설명을 함께 참조하는 것을 추천한다.   
Repository: [https://github.com/YongWookHa/django-rest-image-upload](https://github.com/YongWookHa/django-rest-image-upload)  

#### 주의 사항
> -Github Repo가 업데이트 됨에 의해, 본 포스팅의 코드와는 약간의 차이가 있을 수 있다.  
> -django 유저들이 만든 API-key 관련 패키지들이 있으니 비교하여 사용하자.  
> -본 코드는 API-key에 대한 암호화 등의 몇가지 중요 기능이 적용되지 않은 코드임에 유의하자. 

## 코드 설명

다시 한번 언급하지만, 아래는 **일부** 코드들에 대한 설명이며, 전체 코드를 함께 참조하기를 당부한다.

---

### 📗 URLS  
#### backend_app/urls.py
```python
urlpatterns = [
    url(r'^', include(('imageupload_frontend.urls', 'imageupload_frontend'), namespace='frontend')),
    url(r'^admin/', admin.site.urls),
    url(r'^api/v1/', include(('imageupload_rest.urls', 'imageupload_rest'), namespace='api')),
]
```
`backend_app`에서는 전체 서버에 대한 설정을 담당한다. 대부분의 설정은 `settings.py`에서 작성되며, `urls.py`에서는 url 설정을 따로 담당한다.  `127.0.0.1/8000`을 로컬 주소라고 할 때, 위 코드가 나타내는 것은 다음과 같다.  
- `127.0.0.1/8000/`으로 접속 -> `imageupload_frontend/urls.py`를 참조하여 서비스로 연결  
- `127.0.0.1/8000/admin`으로 접속 -> `admin`(django 기본 서비스)으로 연결  
- `127.0.0.1/8000/api/v1`으로 접속 -> `imageupload_rest/urls.py`를 참조하여 서비스로 연결  
  

#### imageupload_frontend/urls.py
```python
urlpatterns = [
    url(r'^$', RedirectView.as_view(url='static/index.html', permanent=False), name='index')
]
```
`127.0.0.1/8000/`로 접속하면 `imageupload_frontend/static/index.html'으로 연결

#### imageupload_rest/urls.py
```python
urlpatterns = [
    # url(r'', include(router.urls)),
    url(r'^images', UploadedImagesViewSet.as_view({'get': 'list', 'post': 'post_images'}), name='images'),
    url(r'^base64', UploadedImagesViewSet.as_view({'get': 'list', 'post': 'post_base64'}), name='base64')
]
```
`127.0.0.1/8000/api/v1/images`로 접속하면 `UploadImagesViewSet`으로 연결  
`127.0.0.1/8000/api/v1/base64`로 접속해도 `UploadImagesViewSet`으로 연결  

사용자가 `127.0.0.1/8000/api/v1/images`와 `127.0.0.1/8000/api/v1/base64`에서 `get` request를 하면 UploadedImagesViewSet의 `list` method로 연결하고 `post` request를 하면 각각 `post_images`와 `post_base64` method(이후 설명)로 연결한다.

---

### 💾 MODELS  
> *django에서 model은 DB에 저장되는 Table을 말한다.*  

#### user_model_customize/models.py  
```python
class User(AbstractUser):    
    objects = UserManager()
    api_key = models.UUIDField(unique=True, default=uuid.uuid4, editable=False)  

    def __str__(self):
        return self.username
```
Django가 제공하는 기본 User 모델에서 `api_key` feature만 추가한다. `default`와 `editable`인자를 위와 같이 설정하므로써, User 생성시에 최초 1회 API-key가 발급되고, 이는 변경되지 않는다.  

#### imageupload/models.py
```python
def scramble_uploaded_filename(instance, filename):
    extension = filename.split(".")[-1]
    return "{}.{}".format(instance.image_id, extension)

class UploadedImage(models.Model):
    image_id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    image = models.ImageField("Uploaded image", upload_to=scramble_uploaded_filename)
    owner = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_date = models.DateTimeField(auto_now_add=True, editable=False)
```
UploadedImage는 위와 같은 field들을 가진다. `image_id`는 UUID로 생성되고, 이미지의 `filename`은 `image_id`를 참조하여 새로 결정된다.

---

### 🧾 SERIALIZERS
> *Serializer는 model을 출력할 때, 출력 형식을 어떻게 할지에 대한 설정이다.*
> *API request의 형식의 validation을 이곳에서 진행한다.*  

#### imageupload_rest/serializers.py
```python
class UploadedImageSerializer(serializers.ModelSerializer):
    owner = serializers.SerializerMethodField("get_username")
    api_key = serializers.CharField(write_only=True)
    class Meta:
        model = UploadedImage
        read_only_fields = ['owner', ]
        fields = ['image_id', 'image', 'owner', 'created_date', 'api_key',]  # only serialize the primary key and the image field

    def create(self, validated_data):
        validated_data['owner'] = User.objects.get(api_key=validated_data['api_key'])
        validated_data.pop('api_key', None)
        return super().create(validated_data)

    def get_username(self, obj):
        return obj.owner.username
```
`owner` field는 DB에 owner의 primary key값이 저장되는데, primary key가 아닌, username으로 출력하기 위해 overide한다.  

`imageupload/models.py`의 `UploadedImage` model class를 보면 `api_key` field가 없는데, API request에서 api_key를 입력받아 User와 연결하기 위해 `write_only` 인자를 이용하여 임시 설정한 `api_key` field를 이용한다. 이렇게 model에 저장되지 않는 field를 이용하는 경우, request에 대한 validate 과정에서 `unexpected field` 오류가 발생하기 때문에, `create` method를 overide하여 `validated_data`에서 `api_key`를 제거해주는 작업을 거쳐야 한다.

---

### 📺 VIEWSETS   
> *출력하고 싶은 데이터를 화면에 어떻게 배치할지에 대한 설정이다.*  
> *API request가 validate하다면, model instance를 생성하고 저장하는 것을 이곳에서 진행한다.*   

#### imageupload_rest/viewsets.py   
```python
class UploadedImagesViewSet(viewsets.ModelViewSet):
    queryset = UploadedImage.objects.all()
    serializer_class = UploadedImageSerializer

    def post_images(self, request):
        new_req_dict = dict()
        new_req_dict['owner'] = User.objects.get(api_key=request.data['api_key']).pk
        request.data.update(new_req_dict)

        return self.create(request)
    
    def post_base64(self, request):
        base64img = request.data['image']
        if not isinstance(base64img, str):
            raise ValidationError(
            'Invalid value: {}, expected base64 encoded image'.format(base64img)
            )
        format, imgstr = base64img.split(';base64,')
        ext = format.split('/')[-1]
        new_req_dict = dict()
        new_req_dict['image'] = ContentFile(base64.b64decode(imgstr), name='filename.'+ext)
        new_req_dict['owner'] = User.objects.get(api_key=request.data['api_key']).pk
        request.data.update(new_req_dict)

        return self.create(request)
```
`imageupload_rest/urls.py`에서 `127.0.0.1/8000/api/v1/images`로 접속하여 `post` request를 하면 이 요청을 `post_image` method로 연결한다고 했는데, 이 `post_image` method를 이곳에 선언한다. `UploadedImage` instance의 생성에는 `owner`가 필요한데, 우리는 고유 API-Key로 `owner`를 특정하므로, 이 method에서 `User` object 중에서 request로 들어온 `api_key`를 가진 object를 찾아 그 `User`의 primary key를 request의 `owner` 정보로 입력해준다.  

`post_base64`도 마찬가지로 `127.0.0.1/8000/api/v1/images`에 `post` reqquest를 하는 경우, 이에 base64 encoded image가 들어오는 것으로 간주하고 이를 decode한 후, django의 `ContentFile` method를 이용하여 이미지화하고 저장한다.  

이 코드에서는 request에 포함된 이미지를 포함한 정보들을 DB에 저장하기만 하고 저장 결과(`self.create`의 결과)를 return하는데, 사용시에는 입력으로 들어온 이미지에 대한 서비스도 이곳에서 진행하고 결과를 함께 반환하면 된다.

---

### 🔑 ADMIN  
> *관리자 페이지에서 무엇을, 어떻게 볼지 설정하는 부분이다.*  

#### user_model_customize/admin.py  
```python
class CustomUserAdmin(UserAdmin):
    # fieldsets : 관리자 리스트 화면에서 출력될 폼 설정 부분
    UserAdmin.fieldsets[1][1]['fields']+=('api_key', )
    UserAdmin.readonly_fields+=('api_key', )
    UserAdmin.search_fields+=('api_key',)
    UserAdmin.list_display = ('username', 'email', 'api_key', )
    
    # add_fieldsets : User 객체 추가 화면에 출력될 입력 폼 설정 부분
    UserAdmin.add_fieldsets += (
        (('Additional Info'),{'fields':('email','groups', 'user_permissions')}),
    )
```
직관적인 코드 구성이므로 내부 주석 설명으로도 충분할 듯하다. 

#### Admin에서 Model 확인
DB에 저장될 model 중, admin 페이지에서 확인하길 원하는 데이터가 있다면 `admin.py`에서 admin에 등록하면 된다.  
예를 들어, `imageupload/admin.py`에는 아래와 같은 코드가 포함되어 있다.  
```python
admin.site.register(UploadedImage)
```

---

부족한 코드이지만, 누군가에게는 도움이 되었으면 좋겠다. 😊