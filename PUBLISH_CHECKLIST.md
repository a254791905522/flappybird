# iOS App — App Store 发布规范

> FlappyBird 发布检查清单。

---

## 项目信息

| 字段 | 值 |
|------|-----|
| App 名称 | `FlappyBird` |
| Bundle ID | `com.company.flappybird` |
| 版本号 | `1.0.0` |
| 构建号 | `1` |
| 版权 | `© 2026 Company` |
| App Store Connect Team | `{{TEAM_ID}}` |

---

## 输出文件目录结构

```
publish/flappybird/
├── index.html              # 游戏介绍页（全英文）
├── privacy.html            # 隐私政策页（全英文）
├── PUBLISH_CHECKLIST.md    # 本清单
├── screenshots_65/         # iPhone 6.5寸 (1284×2778)
│   ├── 01_menu.png
│   ├── 02_levels.png
│   ├── 03_gameplay.png
│   ├── 04_gameover.png
│   └── 05_victory.png
└── screenshots_55/         # iPhone 5.5寸 (1242×2208)
    ├── 01_menu.png
    ├── 02_levels.png
    ├── 03_gameplay.png
    ├── 04_gameover.png
    └── 05_victory.png
```

---

## 1. Archive 构建

### 构建命令
```bash
cd /path/to/project
xcodebuild -project {{PROJECT_NAME}}.xcodeproj \
  -scheme {{SCHEME_NAME}} \
  -configuration Release \
  clean archive \
  -archivePath build/{{SCHEME_NAME}}.xcarchive
```

### Archive 检查清单
- [ ] `productName` = `FlappyBird`（project.pbxproj）
- [ ] `PRODUCT_BUNDLE_IDENTIFIER` = `com.company.flappybird`
- [ ] `CFBundleDisplayName` = `FlappyBird`（Info.plist）
- [ ] App Icon 无 Alpha 通道（RGB 模式，非 RGBA）
- [ ] `ITSAppUsesNonExemptEncryption` = false（Info.plist）

### 常见 Archive 错误
| 错误 | 修复 |
|------|------|
| Alpha 通道：`Invalid large app icon...can't be transparent` | `uvx --with pillow python3 -c "from PIL import Image; img=Image.open('icon.png'); bg=Image.new('RGB',img.size,(255,255,255)); bg.paste(img,mask=img.split()[3]); bg.save('icon.png')"` |
| Build 签名失败 | 检查 Xcode Signing & Capabilities，确保 Team 和证书有效 |

---

## 2. 上传方式

### Transporter（推荐）
1. Mac App Store 下载 [Transporter](https://apps.apple.com/us/app/transporter/id1450874784)
2. 打开后拖入 `.xcarchive` 文件
3. 点击 Deliver

### xcodebuild CLI（不可用）
`xcodebuild -exportArchive` 因 CLI 无法访问 Xcode GUI 授权账号，**不可用**。

---

## 3. App Store Connect 填写规范

### ️ App 名称 / Bundle ID 冲突

**创建 App 时常见报错**：
```
App Record Creation failed due to request containing an attribute already in use.
The App Name you entered is already being used.
```

| 错误原因 | 解决方案 |
|---------|---------|
| App 名称已被占用 | 换一个唯一名称（加后缀如 `Pro`、`2`、`- Puzzle Game`） |
| Bundle ID 后缀已被占用 | 换后缀（如 `puzzle` → `puzzlegame`） |
| 同开发者账号内名称重复 | 检查是否在其他 App 中已使用 |

> **建议**：在 App Store Connect 填写前，先搜索确认名称和 Bundle ID 后缀未被占用。

### 基本信息

| 字段 | 填写内容 |
|------|---------|
| App 名称 | `FlappyBird` |
| 副标题 | `Fly Through Pipes` |
| 主语言 | `English` |
| Bundle ID | `com.company.flappybird` |
| 版本号 | `1.0.0` |
| 隐私政策 URL | `https://{{YOUR_DOMAIN}}/privacy.html` |

### 关键词（100字符限制）
```
flappybird,flappy,bird,fly,pipe,arcade,classic,casual,retro,game,fun,reflex,skill,endless
```
> **格式**：逗号分隔，**不加空格**，总长≤100字符。

### 推广文本（170字符）
```
The classic flappy bird game with 50 levels! Tap to fly, dodge the pipes, and beat your high score. Simple, addictive, and fun for everyone!
```

### 描述（4000字符）
```
FlappyBird is the classic arcade-style flying game that everyone loves. Tap to flap your wings, navigate through the pipes, and see how far you can go!

FEATURES:
• 50 carefully designed levels of increasing difficulty
• Classic retro pixel-art style graphics
• Score tracking and best score recording
• Star rating system for each level
• Simple one-tap controls — easy to learn, hard to master
• Day and night visual themes
• No ads, no in-app purchases, no login required

HOW TO PLAY:
1. Tap the screen to flap your wings and fly upward
2. Release to glide downward
3. Navigate through the gaps between green pipes
4. Each pipe passed earns you points
5. Complete levels to unlock new challenges and earn stars

Whether you're a casual player or a competitive gamer chasing high scores, FlappyBird offers endless fun. Download now and start flying!
```

### 版权
```
© 2026 Company
```

### 审核备注
```
This is a single-player arcade game with no login, no ads, no in-app purchases, and no data collection. All game data is stored locally on the device. To test: tap PLAY to start a level, tap the screen to make the bird fly, navigate through pipes to score points. Game Over appears when the bird hits a pipe or the ground.
```

---

## 4. 加密文稿（第1页）

| 字段 | 选择 |
|------|------|
| 你的 App 是否使用加密？ | **否** |

> 完全离线单机游戏填"否"。

---

## 5. 截图规格

### 必填尺寸

| 类型 | 尺寸 | 文件夹 |
|------|------|--------|
| iPhone 6.5寸 | **1284×2778 px** | `screenshots_65/` |
| iPhone 5.5寸 | **1242×2208 px** | `screenshots_55/` |

### 截图内容
1. **主菜单画面** — 展示游戏标题和开始界面
2. **关卡选择画面** — 展示关卡选择界面
3. **核心玩法画面** — 展示小鸟穿越管道的游戏过程
4. **游戏结束画面** — 展示 Game Over 和得分界面
5. **通关胜利画面** — 展示 Level Clear 和星级评价

### 截图处理命令
```bash
cd /Users/wxj/888ios/publish/flappybird

# iPhone 6.5寸 (1284×2778)
uvx --with pillow python3 -c "
from PIL import Image
import os
base = '/Users/wxj/888ios/publish/flappybird'
os.makedirs(os.path.join(base, 'screenshots_65'), exist_ok=True)
files = [
    ('/Users/wxj/Downloads/微信图片_20260428175802_9642_18.png', '01_menu.png'),
    ('/Users/wxj/Downloads/微信图片_20260428182307_9646_18.png', '02_levels.png'),
    ('/Users/wxj/Downloads/微信图片_20260428180920_9644_18.png', '03_gameplay.png'),
    ('/Users/wxj/Downloads/微信图片_20260428180945_9645_18.png', '04_gameover.png'),
    ('/Users/wxj/Downloads/微信图片_20260428175755_9641_18.png', '05_victory.png')
]
for f, n in files:
    img = Image.open(f).resize((1284, 2778), Image.LANCZOS)
    img.save(os.path.join(base, 'screenshots_65', n))
    print(f'Saved screenshots_65/{n}')
"

# iPhone 5.5寸 (1242×2208)
uvx --with pillow python3 -c "
from PIL import Image
import os
base = '/Users/wxj/888ios/publish/flappybird'
os.makedirs(os.path.join(base, 'screenshots_55'), exist_ok=True)
files = [
    ('/Users/wxj/Downloads/微信图片_20260428175802_9642_18.png', '01_menu.png'),
    ('/Users/wxj/Downloads/微信图片_20260428182307_9646_18.png', '02_levels.png'),
    ('/Users/wxj/Downloads/微信图片_20260428180920_9644_18.png', '03_gameplay.png'),
    ('/Users/wxj/Downloads/微信图片_20260428180945_9645_18.png', '04_gameover.png'),
    ('/Users/wxj/Downloads/微信图片_20260428175755_9641_18.png', '05_victory.png')
]
for f, n in files:
    img = Image.open(f).resize((1242, 2208), Image.LANCZOS)
    img.save(os.path.join(base, 'screenshots_55', n))
    print(f'Saved screenshots_55/{n}')
"
```

---

## 6. App 预览视频规格（可选）

（略，按需使用）

---

## 7. 隐私政策页面 (privacy.html)

纯离线单机游戏，声明不收集任何个人信息。详见 `privacy.html`。

---

## 8. 游戏介绍页面 (index.html)

全英文游戏介绍页，包含截图和功能展示。

---

## 9. 提交流程

1. 准备输出文件目录（截图 + index.html + privacy.html）
2. 执行 Archive 构建并验证检查清单
3. 通过 Transporter 上传 `.xcarchive`
4. 登录 [App Store Connect](https://appstoreconnect.apple.com)
5. 我的 App → 新建 App
6. 填写基本信息（名称、Bundle ID、关键词、描述等）
7. 上传截图（6.5寸 + 5.5寸）
8. 填写加密文稿（离线游戏选"否"）
9. 填写隐私政策 URL
10. 提交审核

---

## 10. 快速复制汇总

| 字段 | 内容 |
|------|------|
| **App 名称** | `FlappyBird` |
| **副标题** | `Fly Through Pipes` |
| **Bundle ID** | `com.company.flappybird` |
| **版本号** | `1.0.0` |
| **版权** | `© 2026 Company` |
| **关键词** | `flappybird,flappy,bird,fly,pipe,arcade,classic,casual,retro,game,fun,reflex,skill,endless` |
| **隐私政策** | `https://{{YOUR_DOMAIN}}/privacy.html` |
| **加密** | 否（离线游戏） |
| **审核备注** | `This is a single-player arcade game with no login, no ads, no in-app purchases, and no data collection.` |

---
