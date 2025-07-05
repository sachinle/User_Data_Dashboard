<template>
  <div class="d-flex" style="margin: 0; padding: 0; height: 100vh; overflow: hidden;">

    <!-- Sidebar -->
    <div class="sidebar-premium p-0 vh-100 overflow-hidden" style="width: 320px;">
      <div class="sidebar-header p-4 d-flex align-items-center" style="margin-top: 0; padding-top: 1rem;">
        <div class="logo-badge me-3">
          <i class="bi bi-people-fill"></i>
        </div>
        <div>
          <h5 class="mb-0 text-white" style="font-family: 'Playfair Display', serif; font-weight: 500;">User Directory</h5>
        </div>
      </div>

      <div class="px-3 pb-3 mt-2">
        <div class="search-container position-relative">
          <i class="bi bi-search search-icon"></i>
          <input type="text" class="form-control search-input" placeholder="Search users..." v-model="searchQuery" @input="filterUsers">
        </div>
      </div>

      <div class="user-list-container">
        <div v-if="users.length === 0" class="loading-placeholder p-4 text-center">
          <div class="spinner-border text-light" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          <p class="text-white-50 mt-2">Loading users...</p>
        </div>
        
        <div v-for="user in users" :key="user.id" class="user-card d-flex align-items-center p-3" @click="selectUser(user)" :class="{ 'active-user': selectedUser?.id === user.id }">
          <div class="user-avatar position-relative me-3">
            <img :src="user.image" class="rounded-circle" width="48" height="48" alt="User" />
            <span class="online-status"></span>
          </div>
          <div class="user-info flex-grow-1">
            <div class="d-flex justify-content-between align-items-center">
              <h6 class="mb-0 text-white">{{ user.firstName }} {{ user.lastName }}</h6>
              <small class="text-warning">{{ user.age }}y</small>
            </div>
            <small class="text-white-50 d-block">{{ user.company.title }}</small>
            <div class="user-meta d-flex justify-content-between mt-1">
              <small class="text-primary" style="font-size:12px">
                <i class="bi bi-calendar3 me-1"></i>
                {{ formatDate(user.birthDate) }}
              </small>
              <small class="text-success">
                <i class="bi bi-geo-alt me-1"></i>
                {{ user.address.city }}
              </small>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Main Content -->
    <div class="main-content flex-grow-1 p-0" style="margin-top: 0; height: 100vh; overflow-y: auto;">
      <div class="content-header p-4 border-bottom" style="margin-top: 0; padding-top: 1rem;">
        <h2 class="mb-0" style="font-family: 'Playfair Display', serif;">
          {{ selectedUser ? `${selectedUser.firstName} ${selectedUser.lastName}` : 'User Dashboard' }}
        </h2>
      </div>
      
      <div class="p-4">
        <div v-if="!selectedUser" class="placeholder-content text-center py-5">
          <div class="empty-state">
            <i class="bi bi-person-square placeholder-icon display-4 text-muted"></i>
            <h4 class="mt-3">Select a user</h4>
            <p class="text-muted">Choose a user from the sidebar to view details</p>
          </div>
        </div>
        
        <div v-else class="user-details">
          <!-- User Profile Header -->
          <div class="user-profile-header mb-4 p-4 rounded-3" style="background: linear-gradient(135deg, #4361ee 0%, #3a0ca3 100%);">
            <div class="row align-items-center">
              <div class="col-md-2 d-flex justify-content-center">
                <img :src="selectedUser.image" class="rounded-circle border border-3 border-white shadow" width="120" height="120" alt="User" />
              </div>
              <div class="col-md-10 text-white">
                <h3 class="mb-1">{{ selectedUser.firstName }} {{ selectedUser.lastName }}</h3>
                <p class="mb-2 opacity-75">
                  <i class="bi bi-briefcase me-2"></i>{{ selectedUser.company.title }} at {{ selectedUser.company.name }}
                </p>
                <div class="d-flex flex-wrap gap-2">
                  <span class="badge bg-white text-primary px-3 py-2">
                    <i class="bi bi-gender-ambiguous me-1"></i>{{ selectedUser.gender }}
                  </span>
                  <span class="badge bg-white text-success px-3 py-2">
                    <i class="bi bi-balloon me-1"></i>{{ selectedUser.age }} years
                  </span>
                  <span class="badge bg-white text-dark px-3 py-2">
                    <i class="bi bi-geo-alt me-1"></i>{{ selectedUser.address.city }}
                  </span>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Main Content Cards -->
          <div class="row">
            <!-- Physical Attributes Card -->
            <div class="col-md-6 mb-4">
              <div class="card border-0 shadow-sm h-100">
                <div class="card-header bg-white border-0 py-3 d-flex align-items-center">
                  <i class="bi bi-heart-pulse text-danger fs-4 me-2"></i>
                  <h5 class="mb-0">Physical Attributes</h5>
                </div>
                <div class="card-body">
                  <div class="attribute-list">
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-primary rounded-3 p-3 me-3">
                        <i class="bi bi-arrow-up text-primary fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Height</small>
                        <h6 class="mb-0">{{ selectedUser.height }} cm</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-success rounded-3 p-3 me-3">
                        <i class="bi bi-speedometer2 text-success fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Weight</small>
                        <h6 class="mb-0">{{ selectedUser.weight }} kg</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-danger rounded-3 p-3 me-3">
                        <i class="bi bi-droplet text-danger fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Blood Group</small>
                        <h6 class="mb-0">{{ selectedUser.bloodGroup }}</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-warning rounded-3 p-3 me-3">
                        <i class="bi bi-eye text-warning fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Eye Color</small>
                        <h6 class="mb-0">{{ selectedUser.eyeColor }}</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-info rounded-3 p-3 me-3">
                        <i class="bi bi-scissors text-info fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Hair</small>
                        <h6 class="mb-0">{{ selectedUser.hair.color }}, {{ selectedUser.hair.type }}</h6>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Contact Details Card -->
            <div class="col-md-6 mb-4">
              <div class="card border-0 shadow-sm h-100">
                <div class="card-header bg-white border-0 py-3 d-flex align-items-center">
                  <i class="bi bi-person-lines-fill text-primary fs-4 me-2"></i>
                  <h5 class="mb-0">Contact Details</h5>
                </div>
                <div class="card-body">
                  <div class="attribute-list">
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-primary rounded-3 p-3 me-3">
                        <i class="bi bi-envelope text-primary fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Email</small>
                        <h6 class="mb-0">{{ selectedUser.email }}</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-success rounded-3 p-3 me-3">
                        <i class="bi bi-telephone text-success fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Phone</small>
                        <h6 class="mb-0">{{ selectedUser.phone }}</h6>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-start p-3">
                      <div class="attribute-icon bg-light-warning rounded-3 p-3 me-3">
                        <i class="bi bi-house-door text-warning fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Address</small>
                        <div class="address-text">
                          {{ selectedUser.address.address }},<br>
                          {{ selectedUser.address.city }}, {{ selectedUser.address.state }}<br>
                          {{ selectedUser.address.postalCode }}
                        </div>
                      </div>
                    </div>
                    
                    <div class="attribute-item d-flex align-items-center p-3">
                      <div class="attribute-icon bg-light-info rounded-3 p-3 me-3">
                        <i class="bi bi-calendar3 text-info fs-5"></i>
                      </div>
                      <div>
                        <small class="text-muted d-block">Birth Date</small>
                        <h6 class="mb-0">{{ formatDate(selectedUser.birthDate) }}</h6>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      allUsers: [],
      selectedUser: null,
      searchQuery: ''
    };
  },
  mounted() {
    fetch('https://dummyjson.com/users')
      .then(res => res.json())
      .then(data => {
        this.users = data.users;
        this.allUsers = data.users; 
      })
      .catch(err => console.error('Fetch error:', err));
  },
  methods: {
    formatDate(dateStr) {
      const date = new Date(dateStr);
      return date.toLocaleDateString('en-IN', {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      });
    },
    selectUser(user) {
      this.selectedUser = user;
    },

    filterUsers() {
      if (!this.searchQuery) {
        this.users = this.allUsers;
        return;
      }
      
      const query = this.searchQuery.toLowerCase();
      this.users = this.allUsers.filter(user => {
        return (
          user.firstName.toLowerCase().includes(query) ||
          user.lastName.toLowerCase().includes(query)
        );
      });
    }
  }
};
</script>

<style scoped>

.user-profile-header {
  box-shadow: 0 4px 20px rgba(67, 97, 238, 0.15);
  transition: all 0.3s ease;
}

.user-profile-header:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 25px rgba(67, 97, 238, 0.2);
}

.card {
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1) !important;
}

.attribute-list {
  border-radius: 10px;
  overflow: hidden;
}

.attribute-item {
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
}

.attribute-item:last-child {
  border-bottom: none;
}

.attribute-item:hover {
  background-color: rgba(0, 0, 0, 0.02);
}

.attribute-icon {
  transition: all 0.3s ease;
}

.attribute-item:hover .attribute-icon {
  transform: scale(1.1);
}

.bg-light-primary {
  background-color: rgba(13, 110, 253, 0.1) !important;
}

.bg-light-success {
  background-color: rgba(25, 135, 84, 0.1) !important;
}

.bg-light-danger {
  background-color: rgba(220, 53, 69, 0.1) !important;
}

.bg-light-warning {
  background-color: rgba(255, 193, 7, 0.1) !important;
}

.bg-light-info {
  background-color: rgba(13, 202, 240, 0.1) !important;
}

.empty-state {
  padding: 3rem;
  background-color: rgba(0, 0, 0, 0.02);
  border-radius: 12px;
  border: 1px dashed rgba(0, 0, 0, 0.1);
}

.address-text {
  line-height: 1.5;
}


.active-user {
  background: rgba(255, 255, 255, 0.1) !important;
  border-left: 3px solid #4361ee !important;
}

.user-details {
  animation: fadeIn 0.3s ease;
}



.user-details {
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.attribute-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid rgba(0,0,0,0.05);
}

.attribute-item span:first-child {
  color: #6c757d;
  font-weight: 500;
}

.badge {
  font-weight: 500;
  padding: 5px 10px;
}
</style>

<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500&display=swap');
@import url('https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css');


body > * {
  margin: 0 !important;
  padding: 0 !important;
}
</style>

<style scoped>
.sidebar-premium {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  box-shadow: 5px 0 15px rgba(0, 0, 0, 0.1);
  margin-top: 0 !important;
}

.sidebar-header {
  background: rgba(255, 255, 255, 0.05);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 0 !important;
}

.logo-badge {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #4361ee 0%, #3a0ca3 100%);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 1.2rem;
}

.search-container {
  position: relative;
}

.search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: rgba(255, 255, 255, 0.6);
  z-index: 2;
}

.search-input {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: white;
  padding-left: 40px;
  border-radius: 8px;
  height: 38px;
}

.user-list-container {
  height: calc(100vh - 150px);
  overflow-y: auto;
  padding-bottom: 20px;
}

.user-list-container::-webkit-scrollbar {
  width: 6px;
}

.user-list-container::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 3px;
}

.user-list-container::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
}

.user-card {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
  margin: 0 12px;
  border-radius: 8px;
}

.user-card:hover {
  background: rgba(255, 255, 255, 0.05);
  transform: translateX(4px);
}

.user-avatar {
  position: relative;
}

.online-status {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 12px;
  height: 12px;
  background-color: #06f506;
  border: 2px solid #1a1a2e;
  border-radius: 50%;
}

.main-content {
  background-color: #f8f9fa;
  margin-top: 0 !important;
}

.content-header {
  background: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  margin-top: 0 !important;
}
</style>