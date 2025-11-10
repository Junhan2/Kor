# Fancy Components 한글 가이드

## 📚 프로젝트 소개

**Fancy Components**는 웹을 다시 재미있게 만들기 위한 애니메이션 React 컴포넌트 및 마이크로인터랙션 라이브러리입니다. 무료 오픈소스로 제공됩니다.

- **공식 문서**: https://fancycomponents.dev/docs/introduction
- **라이센스**: MIT
- **영감**: shadcn-ui의 구조, 레지스트리 시스템, 가이드 등을 기반으로 구축

---

## 🎯 프로젝트 철학

Fancy Components는 GitHub에 존재하는 많은 우수한 컴포넌트 라이브러리들과는 다른 접근 방식을 취합니다:

- **순수한 유틸리티보다 창의성과 실험을 우선**
- **독창적이고 색다른 컴포넌트와 마이크로인터랙션에 집중**
- 기존 카탈로그를 둘러보고 방향성을 파악하세요
- 아이디어가 이 방향과 맞는지 확실하지 않다면 언제든지 문의하세요

---

## 🛠️ 기술 스택

### 필수 스택 (컴포넌트 작성 시)
- **React 18** (React 19는 아직 미지원)
- **TypeScript**
- **Tailwind CSS v4**
- **Motion** (최신 `motion` 패키지 사용, `framer-motion` ❌)

### 사이트 전체 스택
- **프레임워크**: Next.js 15
- **문서**: MDX
- **CMS**: Contentful (썸네일 및 데모 비디오)

---

## 📁 프로젝트 구조

```
src/
├── app/               # Next.js 애플리케이션
├── components/        # 웹사이트 UI 컴포넌트 (fancy 제외)
├── content/           # 컴포넌트 문서 콘텐츠
└── fancy/             # 레지스트리, 소스 코드, 데모
    ├── components/    # 실제 컴포넌트 소스
    └── examples/      # 컴포넌트 데모
```

| 경로 | 설명 |
|------|------|
| `src/app` | 웹사이트용 Next.js 애플리케이션 |
| `src/components` | 웹사이트 React 컴포넌트 (fancy 컴포넌트 제외) |
| `src/content` | 웹사이트 콘텐츠 및 컴포넌트 문서 |
| `src/fancy` | fancy 컴포넌트의 레지스트리, 소스 코드, 데모 |

---

## 🚀 시작하기

### 1. 저장소 포크

GitHub 페이지 우측 상단의 fork 버튼을 클릭하세요.

### 2. 로컬에 클론

```bash
git clone https://github.com/your-username/fancy.git
```

### 3. 프로젝트 디렉토리로 이동

```bash
cd fancy
```

### 4. 새 브랜치 생성

```bash
git checkout -b my-new-branch
```

### 5. 의존성 설치

```bash
pnpm install
```

### 6. 레지스트리 빌드

문서에서 컴포넌트에 접근하려면 이 작업이 필수입니다:

```bash
pnpm run build:registry
```

이제 개발 준비가 완료되었습니다!

### 7. 개발 서버 실행

```bash
pnpm dev
```

---

## ✨ 컴포넌트 추가하기

컴포넌트를 추가하기 전에 **반드시** 아래의 수용 기준을 검토하고 이해하세요.

### 📋 컴포넌트 수용 기준

#### 1. 크레딧 및 저작권

- **1:1 복사 금지**: 다른 사람의 작업을 그대로 복사하지 마세요
- **재창조는 OK**: 컨셉 자체를 재구현하는 것은 괜찮지만, 데모를 새로운 방식으로 패키징하세요
- **널리 알려진 컨셉**: 일반적인 호버 효과 같은 경우 원작자 추적이 불가능하므로 크레딧 불필요
- **특정 작품 기반**: 누군가의 특정 작업에서 영감을 받았다면 반드시 출처를 밝히세요
- **크레딧 위치**:
  - 컴포넌트 문서 페이지 하단
  - 쇼케이스 데모 하단
  - 페이지 상단에 작성자 크레딧도 추가

**⚠️ 주의**: 1:1 복사이거나 적절한 출처 표시가 없는 컴포넌트는 거부될 수 있습니다. 또한 원작자의 정당한 요청이 있을 경우 병합 후에도 제거될 수 있습니다.

소스 코드에도 동일한 원칙이 적용됩니다.

#### 2. 성능

컴포넌트는 합리적인 성능을 보여야 합니다:

- **MotionValues 활용**: `motion`의 MotionValues 사용
- **애니메이션 최적화**: `useEffect` 대신 `useAnimationFrame` 훅 사용 (`motion` 제공)
- **리렌더링 최소화**: 불필요한 재렌더링 방지

**성능 테스트 도구**: [react-scan](https://github.com/aidenybai/react-scan) 권장

#### 3. 접근성

아직 개선할 부분이 많지만, 다음 기본 사항은 구현하세요:

- Tailwind의 `sr-only` 클래스 사용 (스크린 리더용 콘텐츠)
- 장식 요소에 `aria-hidden` 적용
- 기본적인 접근성 구현 예시는 기존 텍스트 컴포넌트 참조

#### 4. 데모

- **최소 1개 필수**: 컴포넌트 작동을 보여주는 데모 최소 1개 필요
- **색상**: [tailwind.config.ts](./tailwind.config.ts)에 사이트 일관성을 위한 색상 클래스 제공
- **창의성 환영**: 자유롭게 창작하되, 모든 뷰포트 크기에서 잘 작동해야 함
- **시각적 매력**: 그래픽 디자이너처럼 '엣지있게' 표현해도 OK

#### 5. 문서화

컴포넌트 문서 페이지에 포함되어야 할 내용:

- 명확한 사용 방법
- 컴포넌트 이해하기 섹션 (복잡한 구현의 경우)
- 다양한 시나리오나 props를 사용한 예제
- 제한 사항이나 주의사항 (해당되는 경우)
- 완전한 props 문서
- 출처 표시 (해당되는 경우)

---

### 📝 컴포넌트 추가 단계별 가이드

#### 1단계: 컴포넌트 소스 작성

1. `src/fancy/components` 디렉토리로 이동
2. 카테고리 선택 또는 생성: `text`, `background`, `blocks` 등
3. 선택한 폴더에 컴포넌트 생성
   - 올바른 [기술 스택](#필수-스택-컴포넌트-작성-시) 사용
   - 주석은 권장되지만 필수는 아님
4. 새 의존성 사용 시 `package.json`에 설치
5. 새 hooks 사용 시 `src/hooks` 디렉토리에 추가
6. 새 유틸리티 사용 시 `src/utils` 디렉토리에 추가

**⚠️ 중요**: hooks와 utilities를 import할 때 반드시 `@/hooks`, `@/utils` path alias 사용:

```tsx
// ✅ 올바름
import { useHook } from "@/hooks/use-hook"

// ❌ 잘못됨
import { useHook } from "../../hooks/use-hook"
```

레지스트리 빌드 스크립트가 이 특정 import 경로를 찾아 필요한 파일과 의존성을 결정하기 때문입니다. 잘못된 경로를 사용하면 CLI 설치가 제대로 작동하지 않습니다.

7. **작성자 표시 필수**: 모든 파일 최상단에 다음 형식으로 작성자 명시

```tsx
// author: author_1 <https://x.com/author_1>, author_2 <https://x.com/author_2>
```

[예시 참고](./src/fancy/components/blocks/stacking-cards.tsx)

#### 추가 설정 (선택사항)

소스 파일에서 가져올 수 없는 정보는 컴포넌트와 동일한 이름의 `.json` 파일로 제공:

- CSS 변수 (해당되는 경우)
- 컴포넌트의 dev dependencies (예: matter-js의 타입)
- 추가 dependencies (소스에는 없지만 컴포넌트 작동에 필요한 것)

스키마: [registry schema file](./src/fancy/schema.ts)

**예시**:
- [CSS 설정 예시](./src/fancy/components/blocks/circling-elements.json)
- [추가 의존성 예시](./src/fancy/components/text/gravity.json)

#### 2단계: 컴포넌트 데모 작성

1. `src/fancy/examples` 디렉토리로 이동
2. 컴포넌트 소스와 동일한 카테고리 폴더에 데모 파일 생성
3. 파일명: 컴포넌트 이름을 kebab-case로, 이상적으로는 `-demo` 접미사
4. 여러 데모 생성 가능 (다양한 사용 사례나 변형 표시)

#### 3단계: 소스 생성

```bash
pnpm run build:registry
```

이 스크립트는 다음을 생성합니다:
- 컴포넌트 문서용 소스 파일
- 레지스트리 인덱스 파일

이 작업이 없으면:
- 문서에서 컴포넌트 참조 불가
- 소스 코드 확인 불가
- CLI 설치 작동 불가

#### 4단계: 컴포넌트 문서 작성

1. `src/content/docs/components` 디렉토리로 이동
2. 컴포넌트 소스와 동일한 카테고리 폴더에 `.mdx` 파일 생성
3. 컴포넌트 헤더에 title, description 추가
4. 작성자 정보 추가 (웹사이트나 프로필 링크 포함 권장)

**헤더 예시**:

```mdx
---
title: Letter Swap Hover
description: A text component that swaps the letters vertically on hover.
component: true
author: johndoe <https://example.com>
---
```

**문서 구조**:

5. 헤더 다음 첫 노드: `ComponentPreview` 컴포넌트 (메인 데모 렌더링)

6. **Installation** 헤더:
   - `Tabs` 컴포넌트 사용 (두 탭: `CLI`, `Manual`)
   - `CLI` 탭: `InstallTabs` 컴포넌트로 래핑된 설치 명령어
     ```
     npx shadcn add @fancy/{component-name}
     ```
   - `Manual` 탭: `ComponentSource` 컴포넌트에서 참조된 소스 코드

7. hooks나 다른 의존성이 필요하면 해당 섹션에 `ComponentSource` 컴포넌트로 포함

8. **Usage** 및/또는 **Understanding the component** 섹션 (해당되는 경우)

9. **Examples** 섹션 (해당되는 경우)

10. **Notes** 섹션 (해당되는 경우)

11. **Props** 섹션: `Table` 컴포넌트 사용

12. **Credits** 섹션 (해당되는 경우): 각 데모 하단에도 크레딧 포함

[기존 컴포넌트 예시](./src/content/docs/components/blocks/circling-elements.mdx) 참고하세요.

#### 5단계: 네비게이션 업데이트

`src/config/docs.ts` 파일에서 `docsConfig` 배열의 올바른 카테고리에 새 항목 추가:

```ts
{
  title: "Component Name",
  href: "/docs/components/{category}/{component-name}",
  label: "New"
}
```

#### 6단계: PR 제출

- 작업 내용을 빠르게 녹화한 영상을 PR 설명에 업로드하세요
- 리뷰 프로세스 속도를 높이는 데 도움이 됩니다
- [예시 PR](https://github.com/danielpetho/fancy/pull/2) 참고

막히는 부분이 있다면 언제든지 도움을 요청하세요!

---

## 📜 커밋 컨벤션

Pull Request를 생성하기 전에 커밋이 다음 컨벤션을 따르는지 확인하세요:

**형식**: `category(scope or module): message`

**카테고리**:
- `feat` / `feature`: 완전히 새로운 코드나 기능
- `fix`: 버그 수정 (가능하면 이슈 번호 참조)
- `refactor`: 수정이나 기능이 아닌 코드 관련 변경
- `docs`: 문서 변경 또는 생성
- `build`: 빌드 소프트웨어, 의존성 변경 또는 추가
- `test`: 테스트 관련 모든 변경
- `ci`: CI 설정 관련 변경 (예: GitHub Actions)
- `chore`: 위 카테고리에 맞지 않는 모든 변경

**예시**:
```
feat(comps): add new prop to the avatar component
```

**규칙**:
- Imperative mood 사용: "add" (O), "added" (X), "adds" (X)
- 첫 글자 대문자 금지
- 끝에 마침표 금지
- 최대 50자
- 구체적이고 실행 가능하게

자세한 사항:
- https://www.conventionalcommits.org/
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)

---

## 💡 새 컴포넌트 요청

새 컴포넌트에 대한 요청이 있다면 GitHub에서 discussion을 열어주세요. 기꺼이 도와드리겠습니다.

---

## 🆘 도움이 필요하신가요?

- **daniel에게 연락**: [X (Twitter)](https://x.com/nonzeroexitcode)
- **이메일**: hello@danielpetho.com
- **이슈 생성**: GitHub Issues

---

## 🙏 감사의 말

이 저장소의 많은 부분(문서 페이지, 구조, 레지스트리 시스템, 가이드 등)이 [shadcn](https://github.com/shadcn-ui/ui)을 기반으로 구축되었습니다. 감사합니다!

---

## 📄 라이센스

[MIT license](LICENSE)에 따라 라이센스가 부여됩니다.
