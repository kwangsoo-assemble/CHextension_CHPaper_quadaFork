# CHPaper — CommandHelper 확장 포크 (quadaFork)

원본 [PseudoKnight/CHPaper](https://github.com/PseudoKnight/CHPaper)(MIT) 의 포크다. Paper 서버 API 를 MethodScript 함수 · 이벤트로 연다.
GitHub `kwangsoo-assemble/CHextension_CHPaper_quadaFork`(공개 · MIT) — 이 작업본을 그대로 올린다
(하네스 `projects/mc_repositories/tools/repos.py publish chpaper` 로만 · README 한글 원본은 하네스 `readme_ko/CHextension_CHPaper_quadaFork.md`).
원격 `upstream` 이 원본 저장소다 — 원본 이력 위에 우리 커밋이 올라가 있다.

## 원본 대비 바뀐 것

- 새 이벤트 `entity_add_to_world`(Paper `EntityAddToWorldEvent` — `id` · `type` 프리필터 · 데이터) · 공용 CH `entity_data_manager.ms` 가 쓴다
- 이벤트 클래스를 `AbstractGenericEvent<…>` 로(CH 3.3.5 빌드 649 이후 API)
- `PaperListeners.register` 가 이미 등록돼 있으면 먼저 풀고 다시 단다(리로드 때 리스너 중복 방지) · `unregister` 는 등록 전이면 아무것도 안 한다
- `pom.xml` `paper-api` 를 `provided` 로(jar 에 Paper 가 안 들어가게)

## 빌드 · 배포

- `.\mvnw.cmd package`(Maven 미설치 — 래퍼 · `JAVA_HOME` = JDK 21 · `-o` 면 오프라인) → `target/chpaper-<버전>.jar`
- CommandHelper full jar 는 공개 Maven 에 없다 → `libs/` 에 두고 `install:install-file`(com.sk89q:commandhelper:3.3.5-SNAPSHOT:full) · `libs/*.jar` 는 저장소에 안 올린다
- 서버 `plugins/CommandHelper/extensions/` 에 복사(구버전 jar 는 반드시 뺀다 — 중복 로드) · 오프라인 검사용 `plugins/MethodScript/extensions/` 에도 같은 jar
- ⓘ 2026-10-12 서버에 깔린 `chpaper-0.4.4.jar` 는 이 소스의 빌드다 — `0.4.4-but-ai-edited.1` 빌드와 클래스 39개가 같다(매니페스트 버전 줄 · `jarInfo.ser` 만 다름)

## 버전

- `pom.xml` `<version>` 이 정본 — 원본 버전 + 넷째 자리 `.N`(2026-10-23 M41 · 그 전 꼬리 `-but-ai-edited.N` · 코드가 바뀌면 N+1 · 원본 버전이 바뀌면 원본을 따르고 `.1` 부터)
- `Extension.java` `SimpleVersion(0,4,4)` 는 숫자 셋만 받는다 — 원본 숫자를 따른다
- 코드가 바뀌면 버전을 올리고 🎬 뒤 하네스 `repos.py release chpaper`
