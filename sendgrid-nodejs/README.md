# Node.js로 SendGrid 이메일 보내기

이 저장소는 [SendGrid](https://sendgrid.com)를 사용하여 Node.js로 이메일을 보내는 간단한 가이드를 제공합니다. 예제에는 환경 변수 설정, SendGrid 구성, 테스트 이메일 보내기가 포함되어 있습니다.

## 사전 준비 사항

이 가이드를 따르기 위해 필요한 사항:

- [Node.js](https://nodejs.org/)가 컴퓨터에 설치되어 있어야 합니다.
- [SendGrid](https://sendgrid.com) 계정
- SendGrid에서 인증된 발신 이메일 주소
- 인증을 위한 [SendGrid API Key](https://app.sendgrid.com/settings/api_keys)

## 시작하기

### 1단계: Node.js 프로젝트 복제 또는 생성

이 저장소를 복제하거나 직접 Node.js 프로젝트를 생성할 수 있습니다.

```bash
# 이 저장소 복제하기
git clone <repository-url>

# 또는 새 Node.js 프로젝트 생성
mkdir sendgrid-email-sender
cd sendgrid-email-sender
npm init -y
```

### 2단계: 필요한 패키지 설치

`dotenv`와 `@sendgrid/mail` 패키지를 설치해야 합니다:

```bash
npm install dotenv @sendgrid/mail
```

- **dotenv**: 이 패키지를 사용하면 `.env` 파일에서 환경 변수를 사용할 수 있습니다.
- **@sendgrid/mail**: 이 패키지는 SendGrid의 API와 상호작용하여 이메일을 보내는 데 사용됩니다.

### 3단계: 환경 변수 설정

프로젝트의 루트 디렉터리에 `.env` 파일을 생성하여 SendGrid API Key를 저장합니다. 이 키는 SendGrid 인증에 사용됩니다.

```env
SENDGRID_API_KEY=YOUR_SENDGRID_API_KEY
```

`YOUR_SENDGRID_API_KEY`를 실제 SendGrid API Key로 교체하세요. 이 파일은 실수로 공유되지 않도록 `.gitignore`에 추가해야 합니다.

### 4단계: 코드 작성

`sendEmail.js`라는 파일을 생성하고 다음 코드를 추가하세요:

```javascript
require('dotenv').config();
const sgMail = require('@sendgrid/mail');

const apiKey = process.env.SENDGRID_API_KEY;

if (!apiKey) {
    throw new Error('환경 변수에 SENDGRID_API_KEY가 정의되지 않았습니다');
}

sgMail.setApiKey(apiKey);

/**
 * SendGrid를 사용하여 이메일을 보냅니다
 * @param {Object} message - 이메일 세부 정보가 포함된 메시지 객체
 * @param {string} message.from - 발신자 이메일 주소
 * @param {string} message.to - 수신자 이메일 주소
 * @param {string} message.subject - 이메일 제목
 * @param {string} message.text - 이메일의 텍스트 본문
 * @param {string} [message.html] - 이메일의 HTML 본문 (선택 사항)
 */
async function sendEmail(message) {
    try {
        await sgMail.send(message);
        console.log('이메일 전송 성공');
    } catch (error) {
        console.error('이메일 전송 중 오류:', error.message);
        if (error.response) {
            console.error('SendGrid 응답 오류:', error.response.body);
        }
    }
}

// 예시 사용법
const message = {
    from: 'sender@example.com',  // SendGrid에서 인증된 발신자여야 합니다
    to: 'receiver@example.com',
    subject: '테스트 이메일',
    text: '이것은 테스트 이메일입니다',
};

sendEmail(message);
```

### 5단계: 코드 실행

Node.js를 사용하여 스크립트를 실행하세요:

```bash
node sendEmail.js
```

모든 설정이 올바르게 되어 있다면 터미널에 `이메일 전송 성공`이 표시될 것입니다.

## 템플릿을 사용한 이메일 보내기

SendGrid의 동적 템플릿을 사용하여 더 전문적이고 일관된 이메일을 보낼 수 있습니다.

### 템플릿 설정

1. [SendGrid 대시보드](https://app.sendgrid.com/)에 로그인
2. **Email API** > **Dynamic Templates** 메뉴로 이동
3. **Create a Dynamic Template** 클릭
4. 템플릿 이름을 입력하고 생성
5. 템플릿 ID를 복사 (예: `d-xxxxxxxxxxx`)

### 템플릿 이메일 코드

`sendEmailWithTemplate.js` 파일을 사용하여 템플릿 기반 이메일을 보낼 수 있습니다:

```javascript
require('dotenv').config();
const sgMail = require('@sendgrid/mail');

const apiKey = process.env.SENDGRID_API_KEY;

if (!apiKey) {
    throw new Error('환경 변수에 SENDGRID_API_KEY가 정의되지 않았습니다');
}

sgMail.setApiKey(apiKey);

async function sendEmail(message) {
    try {
        await sgMail.send(message);
        console.log('이메일 전송 성공');
    } catch (error) {
        console.error('이메일 전송 중 오류:', error.message);
        if (error.response) {
            console.error('SendGrid 응답 오류:', error.response.body);
        }
    }
}

// 템플릿 사용 예시
const message = {
    from: 'sender@example.com',
    to: 'receiver@example.com',
    templateId: 'd-xxxxxxxxxxx',  // SendGrid에서 생성한 템플릿 ID
    dynamicTemplateData: {
        code: '123456',           // 템플릿에서 사용할 동적 데이터
        username: 'John Doe',
        // 템플릿에 정의된 다른 변수들...
    },
};

sendEmail(message);
```

### 템플릿 실행

```bash
node sendEmailWithTemplate.js
```

## 테스트 실행

이 프로젝트는 Jest를 사용하여 테스트를 실행합니다:

```bash
npm test
```

## 참고 사항

- **API Key 누락**: API 키가 없거나 잘못된 경우 `환경 변수에 SENDGRID_API_KEY가 정의되지 않았습니다`라는 오류가 표시됩니다. `.env` 파일이 올바르게 설정되었는지 확인하세요.
- **인증 문제**: 인증 오류가 있는 경우 API 키를 다시 확인하고 `from` 이메일 주소가 SendGrid 계정에서 인증되었는지 확인하세요.
- **템플릿 ID 오류**: 템플릿 ID가 올바른지 확인하고, 템플릿이 활성화되어 있는지 확인하세요.
- **동적 데이터**: `dynamicTemplateData`의 변수명은 템플릿에서 정의한 변수명과 정확히 일치해야 합니다.

## 프로젝트 파일 구조

```
sendgrid-nodejs/
├── sendEmail.js              # 일반 이메일 발송
├── sendEmailWithTemplate.js  # 템플릿 이메일 발송
├── __tests__/                # 테스트 파일
│   └── index.test.js
├── jest.config.js            # Jest 설정
├── package.json
├── package-lock.json
└── README.md                 # 이 파일
```

---

SendGrid로 즐거운 이메일 보내기! 📧