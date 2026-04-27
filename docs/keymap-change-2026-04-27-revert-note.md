# Keymap変更メモ（2026-04-27）

default レイヤー 左側の3キーを入れ替えた変更の戻し方メモ。

## 変更概要

A・Z・Alt(GRAVE) の3キーを再配置：

- A位置 → Ctrl 単独（タップ・ホールド共にCtrl）
- Z位置 → A（タップでA、ホールドでLeft Shift）
- Alt/GRAVE位置 → Z 単独（タップのみ）

## 変更前の状態（戻すときはこの内容に書き戻す）

`config/LiNEA40.keymap` の `default_layer` bindings の冒頭4行：

```dts
&kp Q             &kp W         &kp E               &kp R    &lt_hp SNIPE T                              &kp Y        &kp U    &kp I                &kp O           &kp P
&mt_bal LCTRL A    &kp S         &kp D               &kp F    &lt_hp 3 G                                    &kp H        &kp J    &kp K                &kp L           &mt_bal RIGHT_CONTROL MINUS
&mt_bal LEFT_SHIFT Z  &kp X         &kp C               &kp V    &kp B                                    &kp N        &kp M    &kp COMMA            &kp DOT         &mt_bal RIGHT_SHIFT SLASH
&lt 3 ESC         &kp LEFT_WIN  &mt LEFT_ALT GRAVE  &kp INS  &lt 4 SPACE  &lt 1 TAB    &kp BACKSPACE  &lt 1 ENTER           &lt 6 RIGHT_BRACKET  &tog 3  &mo 3
```

## 変更後の状態（最終形 / 2回目の入れ替え後）

2段目1列目を A+Shift、3段目1列目を Ctrl 単独に再入れ替え。

```dts
&kp Q             &kp W         &kp E               &kp R    &lt_hp SNIPE T                              &kp Y        &kp U    &kp I                &kp O           &kp P
&mt_bal LEFT_SHIFT A    &kp S         &kp D               &kp F    &lt_hp 3 G                                    &kp H        &kp J    &kp K                &kp L           &mt_bal RIGHT_CONTROL MINUS
&kp LEFT_CONTROL  &kp X         &kp C               &kp V    &kp B                                    &kp N        &kp M    &kp COMMA            &kp DOT         &mt_bal RIGHT_SHIFT SLASH
&lt 3 ESC         &kp LEFT_WIN  &kp Z  &kp INS  &lt 4 SPACE  &lt 1 TAB    &kp BACKSPACE  &lt 1 ENTER           &lt 6 RIGHT_BRACKET  &tog 3  &mo 3
```

### 中間状態（参考 / 1回目の入れ替え時点。コミット f1495fa）

```dts
&kp LEFT_CONTROL    &kp S         &kp D               &kp F    &lt_hp 3 G                                    &kp H        &kp J    &kp K                &kp L           &mt_bal RIGHT_CONTROL MINUS
&mt_bal LEFT_SHIFT A  &kp X         &kp C               &kp V    &kp B                                    &kp N        &kp M    &kp COMMA            &kp DOT         &mt_bal RIGHT_SHIFT SLASH
```

## 戻し方

1. `config/LiNEA40.keymap` を開く
2. `default_layer` の bindings を「変更前の状態」のブロックに戻す
3. ビルド・フラッシュ

## 補足

- LEFT_ALT を default に持つキーがなくなったため、Alt が必要な場合は他レイヤーから供給する想定。必要なら戻すか、別位置に Alt を追加する。
- GRAVE（バッククォート/`）も default から消える。combo `ZenkakuHankaku`(LANG_ZENKAKUHANKAKU, key-positions 11 12) や SIMBOL レイヤーの `&kp GRAVE` で代替可能。
- `mt_bal LEFT_SHIFT A` は他のホームロウ mod と同じ `mt_bal`（balanced, tapping-term-ms=200, quick-tap-ms=175, require-prior-idle-ms=125）を使う。
