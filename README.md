# zmk-config-roBa

<img src="keymap-drawer/roBa.svg" >

## OS切り替え

roBa は **Windows モード**（デフォルト）と **Mac モード**の2つの動作モードを持ちます。  
モードはレイヤー切り替えで永続的に保持されます（電源を切っても維持されます）。

### Windows → Mac への切り替え

1. `LANGUAGE_2`（左人差し指キー）を**長押し** → BTレイヤー（layer 6）が有効になる
2. 右手ホームロウの **`J`** キーを押す → Mac モード（MAC_DEFAULT レイヤー）に切り替わる

### Mac → Windows への戻し方

1. `LANGUAGE_2`（左人差し指キー）を**長押し** → BTレイヤー（layer 6）が有効になる
2. 右手ホームロウの **`H`** キーを押す → Windows モード（デフォルトレイヤー）に切り替わる

> **補足**: BTレイヤーにはBluetooth接続切り替え（`Y`〜`P`）も配置されています。  
> 接続中のデバイスを切り替えたい場合は、同じく `LANGUAGE_2` 長押し後、上段の `Y` / `U` / `I` / `O` / `P` でプロファイル 0〜4 を選択してください。

### Macモードでの主な違い

| 操作 | Windows | Mac |
|------|---------|-----|
| 保存（S+F コンボ） | `Ctrl+S` | `Cmd+S` |
| アンドゥ（X+C コンボ） | `Ctrl+Z` | `Cmd+Z` |
| コメントトグル（J+K コンボ） | `Ctrl+/` | `Cmd+/` |
| コマンドパレット（FUNCTIONレイヤー `Y`） | `Ctrl+Shift+P` | `Cmd+Shift+P` |
| 行削除（FUNCTIONレイヤー `U`） | `Ctrl+Shift+K` | `Cmd+Shift+K` |
| サイドバー（FUNCTIONレイヤー `O`） | `Ctrl+B` | `Cmd+B` |
| ブラウザ戻る（SCROLLレイヤー `A`） | `Alt+Left` | `Cmd+[` |
| タブ閉じる（SCROLLレイヤー `S`） | `Ctrl+W` | `Cmd+W` |
| アドレスバー（SCROLLレイヤー `D`） | `Ctrl+L` | `Cmd+L` |
| リロード（SCROLLレイヤー `F`） | `Ctrl+R` | `Cmd+R` |
| ワード削除（FUNCTIONレイヤー `G`） | `Ctrl+Del` | `Alt+Del` |
| 左サムキー（長押し操作） | スクショ `Win+Shift+S` | スクショ `Cmd+Shift+4` |

---

## クイックリファレンス

### コンボ（デフォルトレイヤー）
| キー | 出力 |
|------|------|
| S + D | Tab |
| D + F | Shift+Tab |
| S + F | Ctrl+S（保存） |
| X + C | Ctrl+Z（アンドゥ） |
| J + K | Ctrl+/（コメント） |
| Q + W | Escape |

### SYMBOLレイヤー（LANGUAGE_1 ホールド）追加記号
| キー | 出力 |
|------|------|
| Q | `->` |
| A | `=>` |
| extra（行2の追加キー） | `!=` |

### FUNCTIONレイヤー右手（LANGUAGE_1 ホールド）
| キー | 操作 |
|------|------|
| Y | Ctrl+Shift+P（コマンドパレット） |
| U | Ctrl+Shift+K（行削除） |
| I | Ctrl+`（ターミナル） |
| O | Ctrl+B（サイドバー） |
| P | Ctrl+Shift+E（エクスプローラー） |
| H | Alt+Shift+F（フォーマット） |
| J | Ctrl+D（次の同一選択） |
| K | Ctrl+F（検索） |
| L | Ctrl+H（置換） |
| エンコーダー | 音量 |

### SCROLLレイヤー左手（K ホールド）
| キー | 操作 |
|------|------|
| W / E / R | 前タブ / 新タブ / 次タブ |
| A | Alt+Left（ブラウザ戻る） |
| S | Ctrl+W（タブ閉じる） |
| D | Ctrl+L（アドレスバー） |
| F | Ctrl+R（リロード） |
| エンコーダー | 水平スクロール |

### MOUSEレイヤー左手（トラックボール動作時）
| キー | 操作 |
|------|------|
| A / S / D | Ctrl / Shift / Alt（修飾キー） |
| Z / X / C / V | Undo / Cut / Copy / Paste |
| エンコーダー | ズームイン/アウト |