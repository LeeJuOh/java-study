### HTTP 기초 CDN이란

- **CDN(Contents Delivery Network) 컨텐츠 전송 네트워크를 뜻한다.**
- 느린 응답속도/ 다운로딩 타임을 극복하기 위한 기술
- 지리, 물리적으로 떨어져있는 사용자에게 컨텐츠를 더 빠르게 제공할 수 있는 기술
- 사용량이 급격히 늘어나서 인터넷 상에서 콘텐츠를 다운로드를 받을 때 콘텐츠 병목 현상이 일어나거나 서버가 다운될 수 있기 때문에, 이럴 때를 대비해서 CDN을 사용함.
- 보통은 원할한 서비스 운영을 위해서 사용됩니다.

CDN을 이해하기 앞서서 어느 업체의 CDN이 유명할까요? 당연히 글로벌 솔루션의 최강자 아마존 웹 서비스이겠지요?

그래서 검색을 해보니, **CloudFront**라고 aws 서비스가 있더군요.

개발자탕구리 탕탕구리 님의 글을 참고해보세요. [[AWS 파헤치기] #2 CloudFront(CDN)가 뭐야?](https://real-dongsoo7.tistory.com/86)

### CDN 사용 사례

쇼핑몰에서 CDN을 사용하는 경우.

> ex2) 예를들어 내 홈페이지가 한국에 있고 내가 사용하던 카페24의 서버에 호스팅되어 있다고 하자.그 호스팅된 공간에 내 홈페이지 이미지가 있고 미국에 거주하는 이용자가 사이트를 접속했을 때 서버는 미국 이용자의 요청을 받아서 이미지를 한국에 있는 서버에서 호출하고 보여주게 된다.하지만 물리적 거리가 멀기 때문에 어느정도의 시간 지연이 발생한다.이를 보완하기 위해 CDN 서비스는 서버 자체를 여러 곳에 두고 이용자가 요청했을때 제일 근접한 서버에서 처리함으로써 지연되는 시간을 줄여 준다.이 과정에서 여러곳에 캐시서버를 분산해서 한 개의 서버가 뻗더라도 다른 서버에서 이미지를 제공할 수 있다.
>

그니까, CDN의 용도는

- 물리적으로 먼 거리에 있는 서버로부터 Client가 Content를 요청할 경우, 빠른 응답을 주기 위해서 origin server의 content를

Client와 가까이 있는 cash 서버에 저장해서 cash 서버가 요청을 응답해 주는 방식

- 쇼핑몰은 이미지를 보여줘야 할 게 많기 때문에, 많은 이미지를 빠르게 보여주기 위해서 이미지를 CDN으로 보냅니다. 이를 **퍼징**이라고 합니다.

### CDN을 사용해야 하는 이유

- 웹 사이트 컨텐츠를 안정적이고 빠르게 사용자에게 웹 상에서 전달하기 위해서 사용해야 합니다.
    - 웹사이트 상에서 Contents(Image, video)의 로딩 속도 개선
    - 컨텐츠 제공의 안정성
    - 보안 개선?

### CDN을 사용하지 않는다면?

다음 처럼 Client의 집중 구타를 얻어 맞을 수 있습니다. 그만큼 트래픽이 금방 server의 한계까지 차오를 것이고, 개발자는 서버가 언제 터질지 조마조마 하면서 생활해야 할 수 있습니다.

![https://blog.kakaocdn.net/dn/oFUXG/btqOvR2aryO/KkfS2DcQwfVYMmNzWAYymK/img.png](https://blog.kakaocdn.net/dn/oFUXG/btqOvR2aryO/KkfS2DcQwfVYMmNzWAYymK/img.png)

출처 : https://en.wikipedia.org/wiki/Content_delivery_network

반면, CDN을 사용한다면,

아래 그림처럼 client의 요청이 여러 CDN 서버로 분산되기 때문에 트래픽에 대한 걱정이 덜해집니다. 그리고 요청된 Client와 지리적으로 가까이 있는 cash server가 요청에 응답을 하기 때문에 WEB상에서의 Contents 전달 속도가 상당히 빨라집니다.

안정적이고 원할한 서비스를 운영할 수 있게 된 겁니다.

![https://blog.kakaocdn.net/dn/InU8u/btqOH9UHxM8/lM7PmldU6AYvBulb63ToG1/img.png](https://blog.kakaocdn.net/dn/InU8u/btqOH9UHxM8/lM7PmldU6AYvBulb63ToG1/img.png)

출처 : https://en.wikipedia.org/wiki/Content_delivery_network

컨텐츠 제공의 안정성은 이 말과 같습니다.

Origin Server 한 대로 무수히 많은 Client의 요청에 이미지와 영상을 미친듯이 보내주는 것보다,

요청하는 Client에 가까이 위치한 cash Server에 미리 컨텐츠를 저장해 두고 이를 전송해 주는 것이 더 빠르고 origin server가 문제가 생겨도 다른 서버가 이를 대처하면 되므로, 서비스의 안정성이 올라갑니다. 그리고 cash server가 전담하는 지역의 클라이언트의 요청만 처리하므로 한 서버로의 트래픽 집중을 방지할 수 있게 됩니다.

### CDN 동작 원리

> 1. 최초 요청에 Client에도 Contents를 전송하지만, 동시에 CDN캐싱장비에도 저장을 합니다.2. 두번째 요청부터는 CDN업체에서 지정하는 해당 컨텐츠 만료 시점까지 CDN 캐싱 장비에 저장된 컨텐츠를 전송합니다.3. 자주 사용하는 페이지에 한해서 CDN장비에서 캐싱되며, 해당 컨텐츠 호출이 없을 경우 주기적으로 삭제됩니다.4. 컨턴츠를 사용할 수 없거나 오래된 경우, CDN은 서버에 대한 요청을 프록시로 작동하여 향후 요청에 대해 응답할 수 있도록 새로운 컨텐츠를 저장합니다.출처: https://goddaehee.tistory.com/173 [갓대희의 작은공간]
>

### CDN 캐싱 방식

1. static cashing

- 개발자가 미리 cash server에 컨텐츠를 미리 올려주는 방식입니다.

2. dynamic cashing

- 클라이언트가 컨텐츠를 요청하면 해당 컨텐츠가 없으면 origin server로부터 다운받아 전달하는 방식입니다.

### CDN 사용 방법

- aws CloudFront
