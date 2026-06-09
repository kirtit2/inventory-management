<template>
  <div class="app-shell">
    <aside class="sidebar" :class="{ collapsed: sidebarCollapsed }">
      <div class="sidebar-header">
        <div class="brand-mark">
          <span class="brand-icon">◈</span>
          <transition name="fade-label">
            <div v-if="!sidebarCollapsed" class="brand-text">
              <h1 class="sidebar-brand">{{ t('nav.companyName') }}</h1>
              <span class="sidebar-subtitle">{{ t('nav.subtitle') }}</span>
            </div>
          </transition>
        </div>
        <button class="collapse-btn" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path
              :d="sidebarCollapsed ? 'M6 4L10 8L6 12' : 'M10 4L6 8L10 12'"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
            />
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }" :title="sidebarCollapsed ? t('nav.overview') : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">{{ t('nav.overview') }}</span>
          </transition>
        </router-link>

        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }" :title="sidebarCollapsed ? t('nav.inventory') : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 5L9 2L16 5V13L9 16L2 13V5Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M9 9V16M9 9L2 5M9 9L16 5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">{{ t('nav.inventory') }}</span>
          </transition>
        </router-link>

        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }" :title="sidebarCollapsed ? t('nav.orders') : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M3 3H5L6.5 10.5H13L15 6H7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <circle cx="7" cy="13.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="12" cy="13.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">{{ t('nav.orders') }}</span>
          </transition>
        </router-link>

        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }" :title="sidebarCollapsed ? t('nav.finance') : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <circle cx="9" cy="9" r="7" stroke="currentColor" stroke-width="1.5"/>
            <path d="M9 5V9L11.5 11.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">{{ t('nav.finance') }}</span>
          </transition>
        </router-link>

        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }" :title="sidebarCollapsed ? t('nav.demandForecast') : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 13L6 9L9 11L13 6L16 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M13 6H16V9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">{{ t('nav.demandForecast') }}</span>
          </transition>
        </router-link>

        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }" :title="sidebarCollapsed ? 'Reports' : undefined">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="3" y="2" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/>
            <path d="M6 6H12M6 9H12M6 12H9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <transition name="fade-label">
            <span v-if="!sidebarCollapsed" class="nav-label">Reports</span>
          </transition>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher :compact="sidebarCollapsed" />
        <ProfileMenu
          :compact="sidebarCollapsed"
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

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

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    // Auto-collapse sidebar on narrow viewports
    const sidebarCollapsed = ref(window.innerWidth < 1024)

    const toggleSidebar = () => {
      sidebarCollapsed.value = !sidebarCollapsed.value
    }

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)
        if (isMockTask) {
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) currentUser.value.tasks.splice(index, 1)
        } else {
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)
        if (mockTask) {
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) apiTasks.value[index] = updatedTask
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(loadTasks)

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      toggleSidebar
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f1f5f9;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ─── App shell ──────────────────────────────────────── */
.app-shell {
  display: flex;
  height: 100vh;
  overflow: hidden;
}

/* ─── Sidebar ────────────────────────────────────────── */
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
  transition: width 0.25s ease, min-width 0.25s ease;
  overflow: hidden;
}

.sidebar.collapsed {
  width: 60px;
  min-width: 60px;
}

/* ─── Sidebar header ─────────────────────────────────── */
.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.125rem 0.875rem 1rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  flex-shrink: 0;
  min-height: 60px;
}

.brand-mark {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  min-width: 0;
  overflow: hidden;
}

.brand-icon {
  font-size: 1.25rem;
  color: #60a5fa;
  flex-shrink: 0;
  line-height: 1;
}

.brand-text {
  min-width: 0;
  overflow: hidden;
}

.sidebar-brand {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #f1f5f9;
  letter-spacing: -0.02em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-subtitle {
  display: block;
  margin-top: 0.125rem;
  font-size: 0.6875rem;
  color: #475569;
  font-weight: 400;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.collapse-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  background: transparent;
  border: none;
  border-radius: 6px;
  color: #475569;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
  flex-shrink: 0;
  padding: 0;
}

.collapse-btn:hover {
  background: rgba(255, 255, 255, 0.08);
  color: #94a3b8;
}

/* ─── Nav ────────────────────────────────────────────── */
.sidebar-nav {
  flex: 1;
  padding: 0.625rem 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 2px;
  overflow-y: auto;
  overflow-x: hidden;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  border-radius: 8px;
  color: #64748b;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s, color 0.15s;
  white-space: nowrap;
  overflow: hidden;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #cbd5e1;
}

.nav-item.active {
  background: rgba(37, 99, 235, 0.18);
  color: #93c5fd;
}

.nav-icon {
  flex-shrink: 0;
  width: 18px;
  height: 18px;
}

.nav-label {
  flex: 1;
}

/* Collapsed: center icons */
.sidebar.collapsed .nav-item {
  justify-content: center;
  padding: 0.625rem;
  gap: 0;
}

/* ─── Sidebar footer ─────────────────────────────────── */
.sidebar-footer {
  padding: 0.75rem 0.625rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex-shrink: 0;
}

/* ─── Main area ──────────────────────────────────────── */
.main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

/* ─── Topbar ─────────────────────────────────────────── */
.topbar {
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
  z-index: 40;
}

/* ─── Page content ───────────────────────────────────── */
.page-content {
  flex: 1;
  overflow-y: auto;
  padding: 1.75rem 2rem;
}

/* ─── Label fade transition ──────────────────────────── */
.fade-label-enter-active,
.fade-label-leave-active {
  transition: opacity 0.15s ease;
}

.fade-label-enter-from,
.fade-label-leave-to {
  opacity: 0;
}

/* ─── Shared page layout ─────────────────────────────── */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #d1fae5; color: #065f46; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fecaca; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #d1fae5; color: #065f46; }
.badge.decreasing { background: #fecaca; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fecaca; color: #991b1b; }
.badge.medium     { background: #fed7aa; color: #92400e; }
.badge.low        { background: #dbeafe; color: #1e40af; }

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>
