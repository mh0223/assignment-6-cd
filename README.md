## <프론트엔드 CI/CD 파이프라인>

## ❓CI/CD란?
- **Continuous Integration**(**지속적 통합**)과 **Continuous Deployment/Delivery(지속적 배포/전달**)
-  소프트웨어 개발 과정에서 코드 변경 사항을 자동으로 통합하고 테스트하며 배포하는 일련의 방법론을 의미합니다.

1. CI 파이프라인 단계
- **코드 병합**: 개발자가 변경된 코드를 중앙 저장소에 푸시합니다.
- **자동 빌드**: 푸시된 코드가 자동으로 빌드됩니다.
- **자동 테스트**: 빌드된 코드에 대해 자동화된 테스트가 실행됩니다.
- **피드백 제공**: 빌드 및 테스트 결과가 개발자에게 즉시 피드백됩니다.
2. 주요 CD 구현 단계
-  **배포 환경 설정**: 개발, 스테이징, 프로덕션 환경을 구성합니다.
- **배포 스크립트 작성**: 애플리케이션을 배포하는 데 필요한 스크립트를 작성합니다.
-  **자동화된 테스트 구현**: 배포 전 수행할 자동화된 테스트를 구현합니다.
-  **배포 파이프라인 구성**: CI 도구와 CD 도구를 연동하여 자동화된 배포 파이프라인을 구성합니다.
-  **모니터링 및 알림 설정**: 배포 후 성능을 모니터링하고 이슈 발생 시 알림을 받을 수 있도록 설정합니다.

## 🛠 GITHUB ACTIONS + AWS
![Blank diagram](https://github.com/user-attachments/assets/b4dd162c-5cc7-4ba7-ab45-73bc25688196)

1. Next.js 프로젝트를 만듭니다.
2. GitHub에 새로운 레포지토리를 만들어, 만들었던 Next.js 프로젝트와 연동합니다.
3. 프로젝트 디렉토리 내에 yml 파일을 작성합니다.

레포지토리에 작업 내용을 push하면, 디렉토리 내의 yml 파일로 인해 GitHub Actions에서 아래와 같이 실행시킵니다.
- 저장소를 체크아웃합니다.
- Node.js 18.x 버전을 설정합니다.
- 프로젝트 의존성을 설치합니다.
- Next.js 프로젝트를 빌드합니다.
- AWS 자격 증명을 구성합니다.
- 빌드된 파일을 S3 버킷에 동기화합니다.
- CloudFront 캐시를 무효화합니다.

## 캐시 무효화 시점
**새로운 파일이 S3 버킷에 성공적으로 업로드된 후에 CloudFront 캐시를 무효화합니다.**

왜 이 시점에서 캐시를 무효화해야 하나요?
1. 최신 콘텐츠 제공:
S3 버킷에 새로운 파일을 업로드하면 CloudFront는 여전히 이전 버전의 파일을 캐시하고 있을 수 있습니다. 따라서, 캐시를 무효화하지 않으면 사용자는 여전히 오래된 파일을 보게 됩니다.
새로운 파일이 업로드된 직후 캐시를 무효화함으로써 사용자에게 최신 버전의 콘텐츠를 제공할 수 있습니다.

2. 일관된 사용자 경험:
최신 파일이 S3에 업로드된 후 캐시를 무효화하면, 모든 사용자가 동일한 최신 콘텐츠를 볼 수 있습니다.
이렇게 하면 콘텐츠 업데이트와 사용자 경험 간의 일관성을 유지할 수 있습니다.

3.문제 해결:
코드나 파일에 중요한 업데이트나 버그 수정이 있는 경우, 캐시를 즉시 무효화하여 문제가 빠르게 해결된 버전을 사용자에게 제공할 수 있습니다.

bucket 웹 사이트 주소: http://mh0223bucket.s3-website-us-east-1.amazonaws.com
cloudfront 웹 사이트 주소: https://d2jtxqe6gz4grz.cloudfront.net
