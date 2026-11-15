# Bếp Việt

Hệ thống Quản lý Chuỗi Nhà Hàng Bếp Việt. Phục vụ vận hành nhiều chi nhánh, quản lý thực đơn, nhân sự, đơn hàng, giao hàng, khách hàng, khuyến mãi, đánh giá và báo cáo.

Chi tiết đặc tả yêu cầu nghiệp vụ xem tại: [Business Requirements](./docs/business-requirements.md).

## Project structure

| Location      | Role                                                       |
| ------------- | ---------------------------------------------------------- |
| `apps/server` | NestJS API backed by PostgreSQL and private media storage  |
| `apps/web`    | Vite + React web app                                       |
| `apps/expo`   | Preserved legacy Expo Router client, managed independently |
| `docs`        | Requirements, technical designs, and development guidance  |

The API and web app use a pnpm workspace. Each app keeps its own version and changelog.

## Documentation

- [Documentation index](./docs/index.md): reading order, document status, and naming conventions.
- [Development guide](./docs/development-guide.md): setup, commands, Docker, environment variables, and releases.
- [Backend design](./docs/technical-design/01-backend.md): architecture, data flows, and implementation limits.
- [Repository rules](./AGENTS.md): engineering conventions and completion checks.
