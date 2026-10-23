# CHextension_CHPaper_quadaFork

> An **AI-assisted fork** of the CommandHelper (MethodScript) extension [CHPaper](https://github.com/PseudoKnight/CHPaper).
> It exposes Paper-only server APIs as MethodScript functions and events. All upstream functions and events are kept; one event is added.

> ⚠ **This is an unofficial fork** and is not affiliated with the original author. The changes were written with an AI coding tool (Claude Code)
> and verified by the maintainer on a live server (Purpur 1.21.8). Please make sure a problem is not caused by this fork before reporting it upstream.

## At a glance

| Item | Value |
|---|---|
| Version | `0.4.4.2` — the fourth number after upstream 0.4.4 is this fork's own release number (first release: tag `0.4.4-but-ai-edited.1`) |
| Based on | upstream `master` `b6eea66` |
| CommandHelper | 3.3.5-SNAPSHOT (build 649 or later — `AbstractGenericEvent` API) |
| Server | Paper-based 1.20.4+ (in use: Purpur 1.21.8) |
| Java | build with 21 (source `release 16`) |

## Changes from upstream

### 1. New event `entity_add_to_world`

Paper's `EntityAddToWorldEvent` — fired any time an entity is added to a world for any reason. It is the counterpart of upstream's `entity_remove_from_world`.

| Field | Content |
|---|---|
| Prefilters | `id` — entity UUID string · `type` — entity type (macro) |
| Event data | `id` — entity UUID · `type` — entity type |
| Mutable fields | none |

```
bind('entity_add_to_world', null, array(type: 'ZOMBIE'), @e) {
    console('A zombie was added: '.@e['id'])
}
```

### 2. Events now extend `AbstractGenericEvent`

Upstream extended `AbstractEvent` and filtered each event with `instanceof`.
They now use CommandHelper 3.3.5's newer `AbstractGenericEvent<…>` API — behaviour is unchanged.

### 3. No duplicate listeners on reload

`PaperListeners.register` unregisters first if it is already registered. `unregister` does nothing if it was never registered.

### 4. `paper-api` is `provided`

The Paper API is not bundled into the built jar.

## Build

The Maven Wrapper is included — no separate Maven install is needed (JDK 21).

CommandHelper is not on a public Maven repository, so install the full jar from Jenkins into your local Maven repository once (the jar in `libs/` — it is not committed):

```
.\mvnw.cmd install:install-file "-Dfile=libs/commandhelper-3.3.5-SNAPSHOT-full.jar" "-DgroupId=com.sk89q" "-DartifactId=commandhelper" "-Dversion=3.3.5-SNAPSHOT" "-Dclassifier=full" "-Dpackaging=jar"
```

Download: https://letsbuild.net/jenkins/job/CommandHelper/lastSuccessfulBuild/artifact/target/commandhelper-3.3.5-SNAPSHOT-full.jar

```
.\mvnw.cmd package
```

Output `target/chpaper-<version>.jar` → put it in the server's `plugins/CommandHelper/extensions/` (remove any old CHPaper jar first — both would load).

## Upstream documentation

The upstream function and event documentation is kept as-is in [`README.upstream.md`](README.upstream.md).

## License

**MIT** (`LICENSE` — unchanged from upstream · Copyright (c) 2019 Michael Smith).

---

# 한국어

> CommandHelper(MethodScript) 확장 [CHPaper](https://github.com/PseudoKnight/CHPaper)를 **AI 도구로 수정한 포크**다.
> Paper 서버에만 있는 API 를 MethodScript 함수 · 이벤트로 연다. 원본 함수 · 이벤트는 그대로 두고 이벤트 하나를 더했다.

> ⚠ **비공식 포크다.** 원본 제작자와 관련이 없다. 수정은 AI 코딩 도구(Claude Code)로 작성했고
> 유지보수자가 실제 서버(Purpur 1.21.8)에서 확인했다. 원본에 문제를 알리기 전에 이 포크 때문인지 먼저 확인해 달라.

## 한눈에

| 항목 | 값 |
|---|---|
| 버전 | `0.4.4.2` — 원본 0.4.4 뒤 넷째 자리가 이 포크의 판 번호(첫 판은 꼬리 `0.4.4-but-ai-edited.1`) |
| 기준 | 원본 `master` `b6eea66` |
| CommandHelper | 3.3.5-SNAPSHOT (빌드 649 이후 — `AbstractGenericEvent` API) |
| 서버 | Paper 계열 1.20.4+ (실사용: Purpur 1.21.8) |
| Java | 빌드 21 (소스 `release 16`) |

## 원본 대비 변경 사항

### 1. 새 이벤트 `entity_add_to_world`

Paper 의 `EntityAddToWorldEvent` — 엔티티가 어떤 이유로든 월드에 더해질 때 불린다. 원본의 `entity_remove_from_world` 짝이다.

| 칸 | 내용 |
|---|---|
| 프리필터 | `id` — 엔티티 UUID 문자열 · `type` — 엔티티 종류(매크로) |
| 이벤트 데이터 | `id` — 엔티티 UUID · `type` — 엔티티 종류 |
| 바꿀 수 있는 값 | 없음 |

```
bind('entity_add_to_world', null, array(type: 'ZOMBIE'), @e) {
    console('좀비가 들어왔다: '.@e['id'])
}
```

### 2. 이벤트 클래스를 `AbstractGenericEvent` 로

원본은 `AbstractEvent` 를 상속하고 이벤트마다 `instanceof` 로 걸렀다.
CommandHelper 3.3.5 의 새 API `AbstractGenericEvent<…>` 로 바꿨다 — 동작은 같다.

### 3. 리로드 때 리스너가 겹치지 않게

`PaperListeners.register` 가 이미 등록돼 있으면 먼저 풀고 다시 단다. `unregister` 는 등록 전이면 아무것도 하지 않는다.

### 4. `paper-api` 를 `provided` 로

빌드 jar 에 Paper API 가 섞이지 않는다.

## 빌드

Maven Wrapper 를 넣어 두었다 — Maven 을 따로 설치하지 않아도 된다(JDK 21).

CommandHelper 는 공개 Maven 저장소에 없으므로, Jenkins 에서 받은 full jar 를 로컬 Maven 저장소에 한 번 설치한다(`libs/` 에 둔 jar — 저장소에는 올리지 않는다):

```
.\mvnw.cmd install:install-file "-Dfile=libs/commandhelper-3.3.5-SNAPSHOT-full.jar" "-DgroupId=com.sk89q" "-DartifactId=commandhelper" "-Dversion=3.3.5-SNAPSHOT" "-Dclassifier=full" "-Dpackaging=jar"
```

받는 곳: https://letsbuild.net/jenkins/job/CommandHelper/lastSuccessfulBuild/artifact/target/commandhelper-3.3.5-SNAPSHOT-full.jar

```
.\mvnw.cmd package
```

산출물 `target/chpaper-<버전>.jar` → 서버 `plugins/CommandHelper/extensions/` 에 넣는다(옛 CHPaper jar 는 반드시 뺀다 — 둘이 같이 로드된다).

## 원본 문서

원본의 함수 · 이벤트 설명은 [`README.upstream.md`](README.upstream.md) 에 그대로 있다.

## 라이선스

**MIT** (`LICENSE` — 원본 그대로 · Copyright (c) 2019 Michael Smith).
