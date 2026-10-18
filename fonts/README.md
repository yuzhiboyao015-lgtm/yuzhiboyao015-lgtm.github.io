# public/fonts

站名「Cody」用的自托管字体。**两个文件都做过子集化。**

| 文件 | 字体 | 用在哪 | 子集 |
|---|---|---|---|
| `cody-script.woff2` | **Great Vibes** | 导航栏 + 首屏（**当前在用**） | 6.01 KB |
| `im-cody-caps.woff2` | Cinzel 700 | 备用，暂未引用 | 1.21 KB |

## 为什么会有两个

文案在几个版本之间来回改过，两份字体分别对应两种排印需求：

- **混大小写**（`Cody` / `i'm cody`）→ Great Vibes 花体
  小写连笔是它的主场，大写只做首字母，正合适。
- **全大写**（`IM CODY`）→ Cinzel 碑铭体
  花体扛不住全大写：它的大写是「装饰性首字母」，原本只用在段落
  开头、单独一个。六个大写连排时进出花尾互相缠住
  （C 的花尾把 O 整个包进去），实测加 0.06em 字距依然读不出字。
  Cinzel 的大写字形就是本职，而且子集更小。

当前是混大小写的 `Cody`，所以用 Great Vibes。
**切换只需改 `globals.css` 里 `.nav-logo-text, .hero-logo` 的
`font-family` 一行**（外加字距/字号那几条，两份字体的度量不同）。

## 来源与许可

两个都是 **SIL Open Font License 1.1**，允许商用、子集化、自托管。

- Great Vibes（当前在用）：`https://fonts.gstatic.com/s/greatvibes/v21/RWmMoKWR9v4ksMfaWd_JN9XFiaQ.woff2`
- Cinzel：`https://fonts.gstatic.com/s/cinzel/v26/8vIU7ww63mVu7gtR-kwKxNvkNOjw-jHgfY3lDQ.woff2`

本地副本在 `.tools/fontwork/`（该目录不进仓库）。

> ⚠️ **css2 接口按 unicode-range 返回多个分片，第一个不是 latin**
> （是 cyrillic-ext，只有 4.4 KB，里面根本没有拉丁字母）。
> 必须按 `/* latin */` 注释定位。这个坑踩过一次。

## 重新子集化

**改动 `site.ts` 的 `logoText` / `heroText` 之后必须重跑**，
否则新字母不会显示（子集里没有那个字形）。

```bash
cd .tools/fontwork
node sub-cinzel.mjs    # Cinzel，字符集在该文件的 CHARSET
node sub.mjs           # Great Vibes，同上
```

用的 `subset-font`（harfbuzzjs 的 WASM 封装），**不需要 Python**。
