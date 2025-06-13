<template>
  <aside :class="['sidebar', { collapsed: isCollapsed }]" aria-label="Main Navigation">
    <div class="logoCompany">
      <img
          src="@/assets/img/logo-TripMatch.png"
          alt="Logo"
          class="logo-img"
          width="40"
          height="40"
      />
      <h1>{{ t('sidevar.logo') }}</h1>
    </div>

    <nav v-if="!isCollapsed" class="content-nav">
      <ul>
        <li v-for="link in links" :key="link.path">
          <RouterLink :to="link.path" class="menu-item">
            <i :class="link.icon" style="margin-right:0.5rem"></i>
            <span>{{ t(link.label) }}</span>
          </RouterLink>
        </li>
      </ul>
    </nav>

    <button class="toggle-button" @click="emit('toggle')">
      {{ isCollapsed ? t('sidevar.show') : t('sidevar.hide') }}
    </button>
  </aside>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue'
import { useI18n } from 'vue-i18n'

const props = defineProps({ isCollapsed: Boolean })
const emit = defineEmits(['toggle'])
const { t } = useI18n()

const isAgency = localStorage.getItem('isAgency') === 'true'

const agencyLinks = [
  { label: 'sidevar.home',             path: '/agency/home',    icon: 'pi pi-home'           },
  { label: 'sidevar.experienceManager',path: '/manageExperience', icon: 'pi pi-objects-column' },
  { label: 'sidevar.reservations',     path: '/reservations',     icon: 'pi pi-pen-to-square'  },
  { label: 'sidevar.queries',          path: '/queries',          icon: 'pi pi-search'         },
  { label: 'sidevar.agencyProfile',    path: '/agency/profile',   icon: 'pi pi-building'       }
]

const touristLinks = [
  { label: 'sidevar.home',        path: '/user/home',   icon: 'pi pi-home' },
  { label: 'sidevar.favorites',   path: '/favorites',   icon: 'pi pi-heart' },
  { label: 'sidevar.itineraries', path: '/itineraries', icon: 'pi pi-calendar' },
  { label: 'sidevar.userProfile', path: '/user/profile', icon: 'pi pi-user' }
]

const links = computed(() => (isAgency ? agencyLinks : touristLinks))
</script>

<style scoped>
.sidebar {
  width: 250px;
  background: #fff;
  border-right: 3px solid #ddd;
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 1rem;
  box-sizing: border-box;
}
.sidebar.collapsed { width: 50px; }

.logoCompany {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.content-nav { flex-grow: 1; }
ul { list-style: none; padding: 0; }
li { margin: 1rem 0; }

.menu-item {
  display: flex;
  align-items: center;
  padding: .5rem 1rem;
  border-radius: 6px;
  text-decoration: none;
  color: #000;
}
.menu-item.router-link-active {
  background: #99D3D2;
  font-weight: bold;
}

.toggle-button {
  background: #2196f3;
  color: #fff;
  border: none;
  padding: .5rem;
  cursor: pointer;
  width: 100%;
}
</style>
