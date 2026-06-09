---
name: vue-sidebar-redesign
description: Redesigns a Vue 3 application from a top navigation bar layout to a modern SaaS-style interface with a vertical sidebar on the left. Use this skill when asked to modernize the UI, convert top-nav to sidebar, or give the app a polished SaaS look.
---

# Vue 3 SaaS Sidebar Redesign

This skill converts a Vue 3 application with a horizontal top-nav into a modern SaaS-style layout with a fixed vertical sidebar on the left. Follow these patterns exactly to produce a consistent, professional result.

## Target Layout

```
┌──────────────────────────────────────────────┐
│ Sidebar (240px) │ Main Content Area           │
│                 │                             │
│ [Logo]          │ [Topbar: filters + profile] │
│ [Nav links]     │                             │
│                 │ [router-view]               │
│                 │                             │
│ [User profile]  │                             │
└──────────────────────────────────────────────┘
```

## Step 1 — Restructure App.vue Template

Replace the existing `<template>` with this structure:

```vue
<template>
  <div class="app-shell">
    <!-- Sidebar -->
    <aside class="sidebar">
      <div class="sidebar-header">
        <h1 class="sidebar-brand">{{ t('nav.companyName') }}</h1>
        <span class="sidebar-subtitle">{{ t('nav.subtitle') }}</span>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }">
          <span class="nav-icon">◈</span>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>
        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }">
          <span class="nav-icon">▦</span>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }">
          <span class="nav-icon">◎</span>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>
        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }">
          <span class="nav-icon">◇</span>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>
        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }">
          <span class="nav-icon">△</span>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }">
          <span class="nav-icon">☰</span>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- Main area: topbar + scrollable content -->
    <div class="main-area">
      <header class="topbar">
        <FilterBar />
      </header>

      <main class="page-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>
```

**Script section is unchanged.** Do not alter any `<script>` logic.

## Step 2 — Replace Global Layout CSS

In the `<style>` (global, not scoped) block in App.vue, **replace only the layout/nav rules**. Keep all shared utility classes (`.badge`, `.card`, `.stat-card`, `.table-container`, `table`, etc.) exactly as they are.

### Layout rules to replace

Remove these old classes completely:
- `.app`, `.top-nav`, `.nav-container`, `.nav-tabs`, `.nav-tabs a`, `.nav-tabs a:hover`, `.nav-tabs a.active`, `.logo`, `.subtitle`, `.main-content`

Add these new classes:

```css
/* ─── Shell ─────────────────────────────────────── */
.app-shell {
  display: flex;
  height: 100vh;
  overflow: hidden;
  background: #f1f5f9;
}

/* ─── Sidebar ────────────────────────────────────── */
.sidebar {
  width: 240px;
  min-width: 240px;
  background: #0f172a;
  display: flex;
  flex-direction: column;
  height: 100vh;
  position: sticky;
  top: 0;
  z-index: 50;
  overflow-y: auto;
}

.sidebar-header {
  padding: 1.5rem 1.25rem 1rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
}

.sidebar-brand {
  font-size: 1.0625rem;
  font-weight: 700;
  color: #f1f5f9;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.sidebar-subtitle {
  display: block;
  margin-top: 0.25rem;
  font-size: 0.75rem;
  color: #64748b;
  font-weight: 400;
}

/* ─── Nav items ──────────────────────────────────── */
.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.875rem;
  border-radius: 8px;
  color: #94a3b8;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s, color 0.15s;
  white-space: nowrap;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #f1f5f9;
}

.nav-item.active {
  background: rgba(37, 99, 235, 0.2);
  color: #93c5fd;
}

.nav-icon {
  font-size: 0.9rem;
  width: 18px;
  text-align: center;
  flex-shrink: 0;
  opacity: 0.8;
}

.nav-label {
  flex: 1;
}

/* ─── Sidebar footer (language + profile) ────────── */
.sidebar-footer {
  padding: 0.875rem 1rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

/* ─── Main area ──────────────────────────────────── */
.main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

/* ─── Topbar (hosts FilterBar) ───────────────────── */
.topbar {
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
  z-index: 40;
}

/* ─── Scrollable page content ────────────────────── */
.page-content {
  flex: 1;
  overflow-y: auto;
  padding: 1.75rem 2rem;
}
```

### Keep the `.page-header` styles unchanged

The `.page-header`, `.page-header h2`, and `.page-header p` rules stay as-is since they live inside views.

## Step 3 — Adapt FilterBar.vue for Topbar Context

The FilterBar now lives in a horizontal topbar instead of a full-width strip below the nav. Update FilterBar.vue styles so it fills the topbar height naturally:

- Remove any `border-bottom` on the FilterBar root (the topbar handles that).
- Set `padding: 0.75rem 1.5rem` on the FilterBar root element.
- Keep all filter dropdowns and their logic completely unchanged.

```css
/* In FilterBar.vue scoped styles — adjust root container */
.filter-bar {
  padding: 0.75rem 1.5rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
}
```

## Step 4 — Design Token Reference

Use these tokens consistently throughout. Do not introduce new color values.

| Token | Value | Usage |
|---|---|---|
| `--sidebar-bg` | `#0f172a` | Sidebar background |
| `--sidebar-text` | `#94a3b8` | Inactive nav text |
| `--sidebar-active-bg` | `rgba(37,99,235,0.2)` | Active nav item |
| `--sidebar-active-text` | `#93c5fd` | Active nav text |
| `--page-bg` | `#f1f5f9` | App background |
| `--topbar-bg` | `#ffffff` | Topbar / FilterBar |
| `--border` | `#e2e8f0` | Dividers, card borders |
| `--text-primary` | `#0f172a` | Headings |
| `--text-muted` | `#64748b` | Secondary labels |
| `--blue-500` | `#2563eb` | Interactive / active |

## Step 5 — Responsive Sidebar (Optional)

For screens below 768px, collapse the sidebar to icon-only (56px) or a mobile drawer. Only implement if the user asks for mobile support.

```css
@media (max-width: 768px) {
  .sidebar {
    width: 56px;
    min-width: 56px;
  }
  .nav-label,
  .sidebar-brand,
  .sidebar-subtitle {
    display: none;
  }
  .nav-item {
    justify-content: center;
    padding: 0.75rem;
  }
  .sidebar-footer {
    flex-direction: column;
    padding: 0.75rem 0.5rem;
  }
}
```

## Step 6 — ProfileMenu & LanguageSwitcher in Dark Context

Both components now render on a dark (`#0f172a`) sidebar background. They likely need style adjustments so text and buttons remain legible. Check for hard-coded light backgrounds or dark text colors:

- LanguageSwitcher: Change button text to `#94a3b8`, hover state to light-on-dark.
- ProfileMenu: Avatar border color and dropdown trigger should use light-on-dark colors.
- Dropdown menus (if any) should still open with white background — they escape the sidebar via `position: fixed` or `z-index`.

## Common Mistakes to Avoid

1. **Do not remove `<script>` logic** — only the `<template>` and layout CSS change.
2. **Do not scope the global styles** — `App.vue` uses `<style>` (no `scoped`) so shared utility classes (`.badge`, `.card`, `.stat-card`) apply globally to all views.
3. **Do not break FilterBar reactivity** — only modify its container padding/border, never touch the ref/watch/composable logic.
4. **Preserve `.page-header`, `.stats-grid`, `.card`, `.badge` rules** — they are used by all view components and must not move or change.
5. **Do not use `position: fixed` for the sidebar** — use `position: sticky; height: 100vh` inside a `height: 100vh; overflow: hidden` shell, which avoids z-index stacking issues.

## Verification Checklist

After making changes, confirm:

- [ ] Sidebar is visible on the left at 240px wide, full viewport height
- [ ] All 6 nav links appear; active link highlights correctly on route change
- [ ] Main content area scrolls independently (sidebar stays fixed)
- [ ] FilterBar renders in the topbar strip without overflow
- [ ] `.stat-card`, `.card`, `.badge` classes still work in view components
- [ ] No horizontal scrollbar at 1280px+ viewport width
- [ ] ProfileMenu dropdown opens correctly (not clipped by sidebar)
- [ ] LanguageSwitcher is legible on dark sidebar background
