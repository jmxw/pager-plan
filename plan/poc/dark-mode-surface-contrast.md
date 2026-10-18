# Dark Mode Surface Contrast Audit

**Repository audited:** `oa-frontend` (the OA admin app served on `localhost:18080`), not
`pager-frontend`. File paths below are relative to the `oa-frontend` repository root. Audit date:
2026-10-18.

## Symptom

On `/#/article/space` (`文档空间`) in dark mode, the `创建新空间` card is painted as a dark block on a
dark surface. The card edge is barely perceivable, and the hover state is invisible. The same
pattern is repeated on other screens.

## Root cause

Two things combine:

1. **The OA dark palette raised the surface but not the tints.**
   `src/styles/appearance-tokens.css` re-aliases, for `html.dark`, the Element Plus (EP) background
   roles onto the OA surface roles (`--el-bg-color` → `--oa-surface` `#1d1e1f`, up from EP's own
   `#141414`). The EP **fill ramp** (`--el-fill-color-*`) and the generated brand **tints**
   (`--el-color-*-light-9`, written by `src/store/modules/settings.ts` as `mix(#141414, color, 0.9)`)
   are left at values designed for a `#141414` surface. Against `#1d1e1f` they collapse to a contrast
   ratio of 1.01–1.11 : 1. One of them (`--el-fill-color-lighter` `#1d1d1d`) is even *darker* than the
   surface, so the ramp is inverted.
2. **Components rely on a tint alone to draw a block.** In light mode a white card on `#f4f4f5`
   (also only 1.10 : 1) still reads as a card because of absolute brightness. In dark mode the same
   ratio reads as nothing. A dark UI needs an **edge** (border ≥ ~1.5 : 1 against the surface) or a
   **shadow**, not a slightly different fill.

### Measured dark contrast against `--oa-surface` (`#1d1e1f`)

| Variable (dark value) | Ratio | Note |
|---|---|---|
| `--el-color-primary-light-9` (`#14202b`, brand `#1890ff`) | 1.01 : 1 | generated tint |
| `--el-fill-color-lighter` (`#1d1d1d`) | 1.01 : 1 | darker than the surface |
| `--el-color-info-light-9` (`#202121`) | 1.03 : 1 | used by the `文档空间` card |
| `--oa-surface-raised` (`#232425`) | 1.07 : 1 | OA role |
| `--el-fill-color-light` (`#262727`) / `--oa-surface-inset` | 1.11 : 1 | |
| `--oa-surface-hover` (`#2a2b2c`) | 1.18 : 1 | |
| `--el-fill-color` (`#303030`) | 1.27 : 1 | |
| `--oa-border-subtle` (`#303235`) | 1.30 : 1 | hairline |
| `--oa-border` (`#3a3d41`) | 1.53 : 1 | visible edge |
| `--oa-border-strong` (`#4c4d4f`) | 1.97 : 1 | strong edge |

Reference: EP's own dark pairing of `--el-fill-color-light` on `--el-bg-color` (`#262727` on
`#141414`) is 1.23 : 1; the OA surface raise halved that margin.

## Findings

All `background` declarations in `src/` whose value is one of the near-surface variables above were
inspected together with the rest of their rule block. Severity: **Broken** = no edge and ≤ 1.05 : 1;
**Weak** = no edge and ≤ 1.3 : 1, or an edge only via a transient state; **OK** = an explicit border or
shadow already provides the edge (listed for completeness, no change required).

### Broken

| Screen (route) | File : line | Element | Fill used | Why it fails |
|---|---|---|---|---|
| `文档空间` (`/article/space`) | `src/views/article/articleSpaces.vue:105` | `.article-space-card` (space cards and the `创建新空间` card) | `--el-color-info-light-9` | 1.03 : 1, no border; the hover `box-shadow` uses the same variable, so hover is invisible too |
| `登记照活动详情` (`/registration-photo/:activityId`) | `src/views/registration-photo/components/ActivityPhotosTab.vue:969`, `:1054` | `.photo-card-placeholder`, `.photo-placeholder` | `--el-fill-color-lighter` | 1.01 : 1 and inverted; placeholder box vanishes inside the card |
| same | `src/views/registration-photo/components/ActivityEnhancedTab.vue:823` | `.photo-card-placeholder` | `--el-fill-color-lighter` | same |
| same | `src/views/registration-photo/components/ActivityOptimizationTab.vue:520` | `.photo-card-placeholder` | `--el-fill-color-lighter` | same |
| `登记照输出详情` (`/registration-photo/:activityId/output/:groupId`) | `src/views/registration-photo/outputGroup.vue:389` | `.output-photo-wrapper` | `--el-fill-color-lighter` | same; the 3 : 4 letterbox area is indistinguishable from the card |

### Weak

| Screen (route) | File : line | Element | Fill used | Why it is weak |
|---|---|---|---|---|
| release notes dialog (global, opened from `src/composables/useReleaseNotesDialog.ts`) | `src/components/ReleaseNotesDialog.vue:187` | `.release-versions` side pane | `--el-fill-color-light` | 1.11 : 1, no divider between pane and body |
| same | `src/components/ReleaseNotesDialog.vue:201` | `.active` version button | `--el-color-primary-light-9` | 1.01 : 1; only the label color changes |
| same | `src/components/ReleaseNotesDialog.vue:257` | `.release-image` / `.image-error` placeholder | `--el-fill-color-light` | 1.11 : 1, no border |
| `看板配置` (`/board/config`) | `src/views/board/config.vue:854`, `:858` | list item `:hover` / `.active` | `--el-fill-color-light`, `--el-color-primary-light-9` | hover 1.11 : 1, active 1.01 : 1; selection state hard to see |
| `个人中心` MCP token panel (`/user/profile`) | `src/views/user/components/McpTokenCreatedDialog.vue:319` | `pre` code block | `--el-fill-color-light` | 1.11 : 1, no border; `--oa-code-bg` exists for this |
| same | `src/views/user/components/McpSetupGuideDrawer.vue:373`, `:384` | `pre`, `.sample--block` | `--el-fill-color-light` | same |
| chat session (`/chat/session/:sessionId`) | `src/views/components/Quote.vue:19` | `.el-quote` | `--el-fill-color-light` | 1.11 : 1, no border or left accent |
| article editor (`/article/:id`) | `src/views/article/component/ArticleTreePanel.vue:134` | `.is-dragging` tree node | `--el-color-primary-light-9` | 1.01 : 1 under 0.5 opacity; drag source not visible |
| `个人中心` MCP token panel (`/user/profile`) | `src/views/user/components/McpTokenPanel.vue:687` | `.token-card` | `--el-fill-color-lighter` + `--el-border-color-lighter` | edge is a 1.30 : 1 hairline; fill is darker than the surface (inverted) |

### OK (edge already present)

| File : line | Element | Edge that saves it |
|---|---|---|
| `src/views/board/components/UserTicketsCard.vue:203`, `BoardStatusTicketsCard.vue:245` | `.ticket-ghost` | `1px dashed var(--el-color-primary)` |
| `src/views/components/SubmitShortcutHint.vue:43` | `kbd` | `1px solid var(--el-border-color)` |
| `src/views/company/components/VendorSettlementSubmissionRail.vue:188`, `:199` | `.card`, `.card.selected` | border + 3 px primary left accent |
| `src/views/company/components/VendorSettlementReviewTabs.vue:489`, `:515` | `.material-text`, `.invoice-surface` | `1px solid var(--el-border-color-lighter)` |

Hard-coded light hexes in `src/views/login/index.vue`, `src/views/wecom/login.vue` and
`src/views/share/vendorSettlement.vue` are intentionally excluded: these routes are in
`FIXED_LIGHT_PATHS` (`src/utils/appearance.ts`) and never render dark.

## Solution

### 1. Component rule: a block on a surface gets an edge, not a tint

The rule that `appearance-tokens.css` already states ("UI surfaces use `--oa-*` roles") is extended
with the dark-mode corollary: **a block that must read as separate from its parent is drawn with a
border (and optionally a shadow); the fill is secondary.** Three recipes cover every finding:

| Intent | Recipe |
|---|---|
| Card / tile on the page surface | `background: var(--oa-surface-raised); border: 1px solid var(--oa-border); border-radius: …; box-shadow: var(--oa-shadow);` hover: `border-color: var(--oa-border-strong); box-shadow: var(--oa-shadow-raised);` |
| Inset content (code, quote, placeholder, side pane) | `background: var(--oa-surface-inset); border: 1px solid var(--oa-border-subtle);` — code blocks use `background: var(--oa-code-bg); color: var(--oa-code-text)` instead; quotes add `border-left: 3px solid var(--oa-border-strong)` |
| Selected / hover / drag state of a list item | `background: var(--oa-surface-selected)` (or `--oa-surface-hover`) **plus** `border-left: 3px solid var(--el-color-primary)` or `outline: 1px solid var(--oa-primary-soft-border)` so the state survives at 1 : 1 fill contrast |

`--el-color-*-light-9` and `--el-fill-color-*` are no longer used as the *only* separator of a block in
`src/views` and `src/components`. Both modes are served by the same declaration: the `--oa-*` roles
and the `.g-card` skin (`src/styles/dashboard-g.scss`) already resolve per mode.

Reference patch for the reported screen (`src/views/article/articleSpaces.vue`):

```css
.article-space-card {
  height: 160px;
  padding: 20px;
  margin-bottom: 20px;
  background-color: var(--oa-surface-raised);
  border: 1px solid var(--oa-border);
  border-radius: 10px;
  box-shadow: var(--oa-shadow);
  cursor: pointer;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.article-space-card:hover {
  border-color: var(--oa-border-strong);
  box-shadow: var(--oa-shadow-raised);
}
```

Per-finding mapping:

| Finding | Recipe |
|---|---|
| `articleSpaces.vue` cards | card recipe (above) |
| registration-photo placeholders ×5 | inset recipe; `.output-photo-wrapper` keeps `overflow: hidden` and adds the hairline only (the photo covers the fill) |
| `ReleaseNotesDialog.vue` side pane | inset fill + `border-right: 1px solid var(--oa-divider)`; `.active` button adds `outline: 1px solid var(--oa-primary-soft-border)` and `background: var(--oa-surface-selected)`; image placeholder gets the inset hairline |
| `board/config.vue` list states | selected/hover recipe with the 3 px left accent |
| MCP `pre` / `.sample--block` ×3 | `--oa-code-bg` / `--oa-code-text` |
| `Quote.vue` | inset recipe with `border-left: 3px solid var(--oa-border-strong)` |
| `ArticleTreePanel.vue` drag node | `outline: 1px dashed var(--el-color-primary)`; keep the opacity |
| `McpTokenPanel.vue` `.token-card` | `background: var(--oa-surface-raised); border-color: var(--oa-border)` |

### 2. Token safety net: re-alias the EP fill ramp in dark mode

So that any `--el-fill-color-*` left behind (and EP's own components) at least keep a monotonic,
non-inverted ramp above the surface, the following aliases are added to the `html.dark` block of
`src/styles/appearance-tokens.css`, next to the existing `--el-bg-color*` aliases:

```css
  --el-fill-color-blank: transparent;
  --el-fill-color-extra-light: var(--oa-surface-raised); /* #232425 */
  --el-fill-color-lighter: var(--oa-surface-inset);      /* #262727, was #1d1d1d (below the surface) */
  --el-fill-color-light: var(--oa-surface-hover);        /* #2a2b2c */
  --el-fill-color: #303236;
  --el-fill-color-dark: var(--oa-border);                /* #3a3d41 */
  --el-fill-color-darker: var(--oa-border-strong);       /* #4c4d4f */
```

This is a safety net only: the resulting ratios stay ≤ 1.3 : 1, so it does **not** replace rule 1.
Because EP components (table stripes, disabled inputs, tag backgrounds, `el-card` headers) consume
the same ramp, the change is reviewed with dark screenshots of a table page, a form page and a
dialog before it is merged. The brand `light-9` tints are left as generated; they are reserved for
"soft" state fills that always carry an edge (rule 1).

### 3. Guard against regressions

A repository check is added (`scripts/check-dark-surfaces.sh`, run in CI next to lint):

```sh
# Fails when a rule block in src/views or src/components paints a background with a near-surface
# tint and the same block declares no border / outline / box-shadow.
grep -rnE 'background(-color)?:\s*var\(--el-(color-[a-z]+-light-(8|9)|fill-color(-light|-lighter|-extra-light)?)\)' \
  --include='*.vue' --include='*.scss' src/views src/components
```

The hit list produced by the command is compared with an allowlist file (`scripts/dark-surfaces.allow`)
that holds only the "OK" rows of this audit; any new hit fails the check until a border is added or the
row is justified in the allowlist.

### 4. Verification

- Dark-mode screenshots are taken of every route in the Broken and Weak tables before and after the
  change; each block edge is expected to reach ≥ 1.5 : 1 against its parent (border) or to carry a
  visible shadow.
- Light mode is screenshotted for the same routes to confirm no regression (the `--oa-*` roles map to
  the previous light values: `--oa-surface-raised` = `#ffffff`, `--oa-surface-inset` = `#f5f7fa`).
- The check in section 3 returns no unlisted hits.
