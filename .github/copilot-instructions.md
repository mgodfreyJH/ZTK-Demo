# ZTK Demo Development Guidelines

Auto-generated from all feature plans. Last updated: 2026-07-30

## Active Technologies
- HTML5, CSS3, ES6+ (ES2020 target — no transpilation) + None (zero external dependencies) (002-paycenter-ztk-demo)
- HTML5 / CSS3 / ES2020 (native browser, no transpilation) + None (003-paycenter-ztk-interactive)
- [if applicable, e.g., PostgreSQL, CoreData, files or N/A] (003-paycenter-ztk-interactive)
- HTML5 / CSS3 / ES2020 (native browser; no transpilation) + None (004-paycenter-ztk-demo-flow)
- N/A — state lives in memory; resets on page reload (004-paycenter-ztk-demo-flow)
- [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION] + [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION] (006-ztk-enroll-p2p-demo)
- ES2020 (native browser — no transpilation) + None (006-ztk-enroll-p2p-demo)
- HTML5, CSS3, JavaScript ES2020 (native browser) + None (zero third-party runtime dependencies) (006-ztk-enroll-p2p-demo)
- N/A (in-memory UI state only) (006-ztk-enroll-p2p-demo)
- HTML5, CSS3, JavaScript ES2020 (native browser — no transpilation, no bundler) + None (zero third-party runtime or build-time dependencies) (006-ztk-enroll-p2p-demo)
- N/A — all state (`activeUseCase`, `flowState`, `currentCarouselIndex`, `isAnimating`, timer handles) lives in memory and resets on page reload (006-ztk-enroll-p2p-demo)

- HTML5, CSS3, ES6+ JavaScript (vanilla — no transpilation needed) + None (001-ztk-demo-carousel)

## Project Structure

```text
src/
tests/
```

## Commands

npm test; npm run lint

## Code Style

HTML5, CSS3, ES6+ JavaScript (vanilla — no transpilation needed): Follow standard conventions

## Recent Changes
- 006-ztk-enroll-p2p-demo: Added HTML5, CSS3, JavaScript ES2020 (native browser — no transpilation, no bundler) + None (zero third-party runtime or build-time dependencies)
- 006-ztk-enroll-p2p-demo: Added HTML5, CSS3, JavaScript ES2020 (native browser) + None (zero third-party runtime dependencies)
- 006-ztk-enroll-p2p-demo: Added ES2020 (native browser — no transpilation) + None


<!-- MANUAL ADDITIONS START -->
<!-- MANUAL ADDITIONS END -->
