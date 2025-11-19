# Email Sender with SendGrid

SendGrid API를 사용하여 Node.js로 이메일을 보내는 프로젝트입니다. 일반 텍스트/HTML 이메일과 템플릿 기반 이메일 발송을 모두 지원합니다.

## 주요 기능

- ✉️ SendGrid API를 사용한 이메일 발송
- 📝 일반 텍스트 및 HTML 이메일 지원
- 🎨 SendGrid 동적 템플릿 지원
- 🔒 환경 변수를 통한 안전한 API 키 관리
- ✅ Jest를 사용한 테스트 환경 구성

## 프로젝트 구조

```
email-sender/
├── sendgrid-nodejs/
│   ├── sendEmail.js              # 기본 이메일 발송 스크립트
│   ├── sendEmailWithTemplate.js  # 템플릿 기반 이메일 발송 스크립트
│   ├── __tests__/                # 테스트 파일
│   ├── package.json
│   └── README.md                 # 상세 가이드 (한국어)
└── README.md                      # 이 파일
```

## 빠른 시작

### 사전 준비 사항

- [Node.js](https://nodejs.org/) (v14 이상 권장)
- [SendGrid](https://sendgrid.com) 계정
- SendGrid API Key
- SendGrid에서 인증된 발신 이메일 주소

### 설치 및 실행

1. **저장소 클론**
   ```bash
   git clone https://github.com/engineer-park/email-sender.git
   cd email-sender/sendgrid-nodejs
   ```

2. **의존성 설치**
   ```bash
   npm install
   ```

3. **환경 변수 설정**

   `.env` 파일을 생성하고 SendGrid API Key를 설정합니다:
   ```env
   SENDGRID_API_KEY=YOUR_SENDGRID_API_KEY
   ```

4. **이메일 발송**

   기본 이메일 발송:
   ```bash
   node sendEmail.js
   ```

   템플릿 이메일 발송:
   ```bash
   node sendEmailWithTemplate.js
   ```

### 테스트 실행

```bash
npm test
```

## 사용 예제

### 기본 이메일 발송

```javascript
const message = {
    from: 'sender@example.com',
    to: 'receiver@example.com',
    subject: 'Test email',
    text: 'This is a test email',
    html: '<strong>This is a test email</strong>'
};

await sendEmail(message);
```

### 템플릿 이메일 발송

```javascript
const message = {
    from: 'sender@example.com',
    to: 'receiver@example.com',
    templateId: 'd-xxxxxxxxxxx',
    dynamicTemplateData: {
        code: '123456',
        username: 'John Doe'
    }
};

await sendEmail(message);
```

## 상세 문서

더 자세한 내용은 [`sendgrid-nodejs/README.md`](./sendgrid-nodejs/README.md)를 참고하세요.

## 기술 스택

- **Node.js** - JavaScript 런타임
- **@sendgrid/mail** - SendGrid 공식 Node.js 라이브러리
- **dotenv** - 환경 변수 관리
- **Jest** - 테스트 프레임워크

## 참고 사항

- API 키는 절대 코드에 직접 포함하지 마세요. 반드시 환경 변수를 사용하세요.
- 발신 이메일 주소는 SendGrid에서 반드시 인증되어야 합니다.
- `.env` 파일은 `.gitignore`에 포함되어 있어 Git에 커밋되지 않습니다.

## 문제 해결

### "SENDGRID_API_KEY is not defined" 오류
- `.env` 파일이 올바른 위치에 있는지 확인
- API 키가 올바르게 설정되었는지 확인

### 인증 오류 (401 Unauthorized)
- SendGrid API 키가 유효한지 확인
- API 키에 메일 발송 권한이 있는지 확인

### 발신자 인증 오류
- SendGrid 계정에서 발신 이메일 주소를 인증했는지 확인

## 라이센스

ISC

## 작성자

finitenumber@gmail.com

## 참고 링크

- [SendGrid 공식 문서](https://docs.sendgrid.com/)
- [SendGrid Node.js 라이브러리](https://github.com/sendgrid/sendgrid-nodejs)
- [SendGrid API 키 관리](https://app.sendgrid.com/settings/api_keys)