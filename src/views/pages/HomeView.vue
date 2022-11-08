<template>
  <div class="home mt-5">
    <div class="container">
      <div class="row justify-content-center text-center">
        <div class="col-8">
          <h4>Find User Github</h4>
          <form v-on:submit.prevent>
            <input
              type="text"
              class="form-control"
              v-model="username"
              placeholder="Search a username github..."
            />
            <button class="btn btn-primary mt-3" @click="getUser">
              Submit
            </button>
          </form>
        </div>
      </div>
      <div class="row h-100 w-100 portofolio my-5" v-show="porto">
        <div class="col-md-4 sidebar text-white pb-3">
          <div class="text-center">
            <img
              v-bind:src="user.avatar_url"
              class="rounded-circle my-5"
              v-bind:alt="user.login"
              height="200"
              width="200"
            />
          </div>
          <h4 class="m-0" v-text="user.name"></h4>
          <h5 class="fw-light" v-text="user.login"></h5>
          <hr class="break-sidebar" />
          <div class="follow d-flex justify-content-start mb-3">
            <i class="bi bi-people"></i>
            <span class="mb-0 mx-1" v-text="user.followers"></span>
            <span>followers</span>
            <span
              class="mx-1 d-flex justify-content-center align-items-center"
              style="font-size: 5px"
              ><i class="bi bi-circle-fill"></i
            ></span>
            <span class="mb-0" v-text="user.following"></span>
            <span class="mx-1">following</span>
          </div>
          <div class="privacy-information">
          <div class="company d-flex justify-content-start" v-if="user.company">
            <i class="bi bi-building"></i>
            <span class="fw-light mb-0 mx-1" v-text="user.company"></span>
          </div>
          <div
            class="location d-flex justify-content-start"
            v-if="user.location"
          >
            <i class="bi bi-geo-alt"></i>
            <span class="fw-light mb-0 mx-1" v-text="user.location"></span>
          </div>
          <div class="blog d-flex justify-content-start" v-if="user.blog">
            <i class="bi bi-globe"></i>
            <span class="fw-light mb-0 mx-1" v-text="user.blog"></span>
          </div>
          <div
            class="twitter d-flex justify-content-start"
            v-if="user.twitter_username"
          >
            <i class="bi bi-twitter"></i>
            <span
              class="fw-light mb-0 mx-1"
              v-text="user.twitter_username"
            ></span>
          </div>
          </div>
        </div>
        <div class="col-md-8 main-page mt-5 mb-0">
          <div class="tab">
            <ul
              class="list-unstyled d-flex flex-wrap justify-content-center mb-0"
            >
              <li
                class="tab-list mx-2"
                :class="[activeTab === 'overview' ? 'active-tab' : '']"
                @click="getOverview"
              >
                Overview
              </li>
              <li
                class="tab-list mx-2"
                :class="[activeTab === 'repositories' ? 'active-tab' : '']"
                @click="getRepositories"
              >
                Repositories
                <span
                  class="badge text-bg-secondary bg-rounded"
                  v-text="user.public_repos"
                ></span>
              </li>
              <li
                class="tab-list mx-2"
                :class="[activeTab === 'projects' ? 'active-tab' : '']"
                @click="getProjects"
              >
                Projects
              </li>
              <li
                class="tab-list mx-2"
                :class="[activeTab === 'packages' ? 'active-tab' : '']"
                @click="getPackages"
              >
                Packages
              </li>
              <li
                class="tab-list mx-2"
                :class="[activeTab === 'stars' ? 'active-tab' : '']"
                @click="getStars"
              >
                Stars
                <span
                  class="badge text-bg-secondary bg-rounded"
                  v-text="totalStarred"
                ></span>
              </li>
            </ul>
            <hr class="m-0" />
          </div>
          <div class="content">
            <!-- OVERVIEW PAGE -->
            <div class="overview" v-if="activeTab === 'overview'">
              <div class="readme-profile mt-3">
                <div class="row">
                  <div class="col-md-12">
                    <div class="card w-100">
                      <div class="card-body">
                        <h5 class="card-title">Card title</h5>
                        <p class="card-text">
                          With supporting text below as a natural lead-in to
                          additional content.
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div
                v-if="totalPinnedRepositories > 0"
                class="pinned-repositories mt-3"
              >
                <div class="row">
                  <div
                    class="col-md-6 mb-2"
                    v-for="(repository, index) in pinnedRepositories"
                    :key="index"
                  >
                    <div class="card w-100">
                      <div class="card-body">
                        <h5 class="card-title">{{ repository.name }}</h5>
                        <p class="card-text">
                          {{ repository.description }}
                        </p>
                        <div class="information d-flex justify-content-start">
                          <div class="language mx-2" v-if="repository.language">
                            <i class="bi bi-circle-fill"></i>
                            <span class="mx-1">{{ repository.language }}</span>
                          </div>
                          <div class="starred mx-2" v-if="repository.stargazers_count">
                            <i class="bi bi-star"></i>
                            <span class="mx-1">{{
                              repository.stargazers_count
                            }}</span>
                          </div>
                          <div class="forked mx-2" v-if="repository.forks">
                            <i class="bi bi-bezier2"></i>
                            <span class="mx-1">{{ repository.forks }}</span>
                          </div>
                        </div>  
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- REPOSITORIES PAGE -->
            <div class="repositories" v-if="activeTab === 'repositories'">
              <div
                v-if="totalRepositories > 0"
                class="pinned-repositories mt-3"
              >
                <div class="row">
                  <div
                    class="col-md-12 mb-2"
                    v-for="(repository, index) in repositories"
                    :key="index"
                  >
                    <div class="card w-100">
                      <div class="card-body">
                        <h5 class="card-title">{{ repository.name }}</h5>
                        <p class="card-text">
                          {{ repository.description }}
                        </p>
                        <div class="information d-flex justify-content-start">
                          <div class="language mx-2" v-if="repository.language">
                            <i class="bi bi-circle-fill"></i>
                            <span class="mx-1">{{ repository.language }}</span>
                          </div>
                          <div class="starred mx-2" v-if="repository.stargazers_count">
                            <i class="bi bi-star"></i>
                            <span class="mx-1">{{
                              repository.stargazers_count
                            }}</span>
                          </div>
                          <div class="forked mx-2" v-if="repository.forks">
                            <i class="bi bi-bezier2"></i>
                            <span class="mx-1">{{ repository.forks }}</span>
                          </div>
                          <div class="updated mx-2" v-if="repository.forks">
                            <span class="mx-1">Updated on {{ new Date((repository.updated_at).split('T')[0]).toString().split(' ').slice(1,4).toString().replaceAll(","," ") }}</span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- PROJECTS PAGE -->
            <div class="projects" v-if="activeTab === 'projects'">Projects</div>
            <!-- PACKAGES PAGE -->
            <div class="Packages" v-if="activeTab === 'packages'">Packages</div>
            <!-- STARS PAGE -->
            <div class="stars" v-if="activeTab === 'stars'">
              <div v-if="totalStarred > 0" class="mt-3">
                <div class="row">
                  <div
                    class="col-md-12 mb-2"
                    v-for="(star, index) in starred"
                    :key="index"
                  >
                    <div class="card w-100">
                      <div class="card-body">
                        <h5 class="card-title">{{ star.name }}</h5>
                        <p class="card-text">
                          {{ star.description }}
                        </p>
                        <div class="information d-flex justify-content-start" >
                          <div class="language mx-2" v-if="star.language">
                            <i class="bi bi-circle-fill"></i>
                            <span class="mx-1">{{ star.language }}</span>
                          </div>
                          <div class="starred mx-2" v-if="star.stargazers_count">
                            <i class="bi bi-star"></i>
                            <span class="mx-1">{{
                              star.stargazers_count
                            }}</span>
                          </div>
                          <div class="forked mx-2" v-if="star.forks">
                            <i class="bi bi-bezier2"></i>
                            <span class="mx-1">{{ star.forks }}</span>
                          </div>
                          <div class="updated mx-2" v-if="star.updated_at">
                            <span class="mx-1">Updated on {{ new Date((star.updated_at).split('T')[0]).toString().split(' ').slice(1,4).toString().replaceAll(","," ")  }}</span>
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
    </div>
  </div>
  {{ getPinnedRepositories }}
  {{ randomColor }}
</template>

<script lang="ts">
import axios from "axios";
import "bootstrap-icons/font/bootstrap-icons.css";

export default {
  name: "home",
  data() {
    return {
      username: "",
      user: [],
      repositories: [],
      totalRepositories: "",
      pinnedRepositories: [],
      totalPinnedRepositories: "",
      starred: [],
      totalStarred: "",
      porto: false,
      content: true,
      activeTab: "overview",
    };
  },
  methods: {
    getAllRepo(user) {
      axios
        .get("https://api.github.com/users/" + user + "/repos")
        .then((response) => {
          this.repositories = response.data;
          this.totalRepositories = response.data.length;
        });
    },
    getStarred(user){
      axios
        .get("https://api.github.com/users/" + user + "/starred")
        .then((response) => {
          this.starred = response.data;
          this.totalStarred = response.data.length;
        });
    },
    getUser() {
      axios
        .get("https://api.github.com/users/" + this.username)
        .then((response) => {
          this.user = response.data;
          this.getAllRepo(this.username);
          this.getStarred(this.username);
          this.porto = true;
          this.getRandomColor();
        })
        .catch((error) => {
          this.porto = false;
        });
    },
    getOverview() {
      this.activeTab = "overview";
      this.getRandomColor();
    },
    getRepositories() {
      this.activeTab = "repositories";
      this.getRandomColor();
    },
    getProjects() {
      this.activeTab = "projects";
      this.getRandomColor();
    },
    getPackages() {
      this.activeTab = "packages";
      this.getRandomColor();
    },
    getStars() {
      this.activeTab = "stars";
      this.getRandomColor();
    },
    getRandomColor(){
      setTimeout(() => {
        const bullet = document.getElementsByClassName('bi-circle-fill');
      console.log(bullet);

      for(let i = 0; i < bullet.length; i++){
        let randomColor = Math.floor(Math.random()*16777215).toString(16);
        console.log(randomColor);
        
        bullet[i].style.color = "#"+randomColor;
      }
      }, 100);
    }
  },
  mounted(){
    
    
  },
  computed: {
    getPinnedRepositories() {
      const repositories = this.repositories;
      const row = [];
      const maxPinned = 6;
      let full = 0;
      if (this.totalRepositories > maxPinned) {
        for (let i = 0; i < this.totalRepositories; i++) {
          if (full != maxPinned) {
            if (repositories[i].fork == false) {
              row.push(repositories[i]);
              full++;
            }
          } else {
            break;
          }
        }
      } else {
        for (let i = 0; i < this.totalRepositories; i++) {
          row.push(repositories[i]);
        }
      }

      this.totalPinnedRepositories = row.length;
      this.pinnedRepositories = row;
    },
  },
};
</script>

<style>
:root {
  --purple-color: #712cf9;
  --accent-color: #0969da;
  --muted-color: rgba(175, 184, 193, 0.2);
}
.portofolio {
  background-color: white;
  border: 1px solid black;
  height: 100%;
  min-height: 100vh;
}

.sidebar {
  background-color: var(--purple-color);
  height: auto;
}

.active-tab {
  border-bottom: 3px solid var(--purple-color);
}

.tab-list:hover {
  cursor: pointer;
}

.card-title {
  color: var(--accent-color);
  font-size: 15px;
}

.card-text {
  font-size: 10px;
}

.break-sidebar {
  border-top: 3px solid #ffffff;
}

.text-bg-secondary {
  background-color: var(--muted-color) !important;
  color: #24292f !important;
}

.information {
  font-size: 12px;
}

.privacy-information{
  font-size: 13px;
}

.bi-circle-fill{
  color: white;
}
</style>