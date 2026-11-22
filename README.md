# MtiennDeptrai Hub — Anime Adventures

Script automation + GUI cho **Anime Adventures (Roblox)**, được xây dựng từ logic đã dịch ngược của `script1.lua` (Luarmor V4 / Luraph v15), `script2.lua` (Prometheus → Elysium Hub + StepLoader), `script3.lua` (MoonSec V3) và dữ liệu `gamedata.txt` (451 remotes, 1.508 scripts).

- **File chạy:** `MtiennDeptrai.lua` (~3.122 dòng, không phụ thuộc link ngoài)
- **Build từ:** `process/build/part_01..07.lua` (xem `process/05_BUILD_LOG.md`)
- **Test:** `luau-ast.exe` (syntax) + `process/smoke_test.lua` (60/60 PASS với mock Roblox)

---

## Cài đặt / Sử dụng

1. Mở Roblox → vào **Anime Adventures**.
2. Chạy executor (Delta / Xeno / Wave / Solara / ...) và paste nội dung `MtiennDeptrai.lua` (hoặc `loadstring` từ file bạn host).
3. Hub hiện sau splash ~2 giây. Nhấn **RightShift** (đổi được trong Settings) để ẩn/hiện. Nút tròn **M** ở góc trái trên để mở lại khi đã đóng.
4. Chọn tính năng ở sidebar; mọi thay đổi được lưu vào `MtiennDeptrai/config.json` (nếu executor hỗ trợ file API).

> Chạy lại script sẽ tự dọn instance cũ (`_G.MtiennDeptrai_Cleanup`) — không bị nhân đôi GUI.

## 7 Tab chức năng

| Tab | Nội dung |
|---|---|
| **Dashboard** | Stats live (Mode, Map, Wave, Yen, Gems, Gold, Trophies, XP, FPS, Ping, Uptime), Quick Farm 1-click, trạng thái engine, teleport lobby nhanh |
| **Auto Play** | Auto Vote Start, Auto Skip Wave, Auto Upgrade (Cheapest / Highest / Farm First / All), Auto Active Ability (buff chain + delay), Auto Priority, Auto Difficulty, Auto Roguelike Card (ưu tiên theo từ khóa), Auto Loot, End-game Replay/Next/Lobby, Auto Leave/Sell tại Wave X |
| **Macro** | Ghi thao tác (hook `__namecall`, fallback phát hiện spawn), phát lại theo Wave/Money/Time, lưu/tải/xóa `MtiennDeptrai/macros/*.json` |
| **Auto Join** | Story (22 world × Act 1–6 × Normal/Hard/Insane, Infinite, Friends Only), Challenge, Daily Challenge, Raid, Portal, Infinity Castle, Tournament, Dungeon Runner (chest/shrine/shop) |
| **Summon & Unit** | Auto Summon (Legacy / EventClover / Summer2026 × gems/ticket × 1/10), Auto Trait Reroll (chọn nhiều trait mục tiêu, **tự dừng khi trúng**), Auto Stat Reroll, Equip Best / Unequip All, Auto Sell Hatched theo rarity |
| **Rewards & Shop** | Claim Daily / BattlePass / Level / First Clear / Missions, Redeem Quests, Redeem Giftcodes hàng loạt, Auto Buy Travelling Merchant / Gold Shop |
| **Settings & Misc** | Discord Webhook (test + auto summary sau trận), Anti-AFK, FPS Booster (Low GFX), Disable 3D (AFK black screen), 10 điểm teleport nhanh, Config Manager (save/load/reset/auto-load), đổi keybind, Save + Destroy Hub |

## Cấu trúc code (trong `MtiennDeptrai.lua`)

1. **Bootstrap & compat** — services, `getgenv`/`gethui`/file API/`request` fallback, cleanup guard.
2. **Config + ConfigManager + GameAPI** — mọi remote bọc `pcall`; `getMyUnits` nhận diện chủ sở hữu 3 tầng (`_stats.player` Object/String/Int, `_stats.player_id`, attribute `Owner`); `placeUnit` tự fallback CFrame ↔ Vector3; `findOpenLobby` đọc `Owner` ObjectValue.
3. **MtiennUI** — zero-dependency, theme Obsidian/Indigo/Emerald, tween, search, toast, floating pill, drag, minimize.
4. **StepLoader splash + Dashboard**.
5. **InMatchEngine + MacroEngine**.
6. **LobbyJoinEngine + SummonTraitEngine + RewardsShopEngine**.
7. **WebhookEngine + OptimizationEngine + Settings + finalize** (auto-load config, `_G.MtiennDeptrai_Hub`, `_G.MtiennDeptrai_Cleanup`).

## Dev workflow (cho AI/người tiếp theo)

```powershell
cd C:\Users\thait\Downloads\scriptmaker2

# 1. sửa part tương ứng trong process\build\  (KHÔNG sửa MtiennDeptrai.lua)
# 2. build + syntax check
python process\build_mtienn.py
# 3. smoke test runtime
.\Deobfuscator-Luraph-V15\bin\luau.exe process\smoke_test.lua
```

- Kế hoạch & tiến độ: `process/00_MASTER_PLAN.md`
- Nhật ký build chi tiết + việc còn lại: `process/05_BUILD_LOG.md`
- Debug nhanh trong game: `_G.MtiennDeptrai_Hub.Config`, `.Flags`, `.Engines`, `.GameAPI`

## Lưu ý

- Không có remote nào bị gọi ngoài UI/engine bạn bật; mặc định mọi thứ **OFF** (trừ UI).
- `spawn_unit` / tên field `_stats` có thể khác giữa các build game — nếu auto không hoạt động, chỉnh `GameAPI.placeUnit` / `getMyUnits` trong `part_02_config_api.lua` rồi build lại.
- Script chỉ dành cho mục đích học tập/reverse-engineering; dùng trong game có thể vi phạm ToS của Roblox.
