<template>
    <header class="HeaderComponent">
      <div class="navbar bg-black shadow-sm p-3" style="z-index: 1; display: flex; justify-content: space-between;">
        <!-- Logo on the left side -->
        <div class="headerPart">
          <RouterLink :to="{ name: 'HomeView'}">
            <img src="../assets/logo.png" alt="Logo" class="logo-img">
          </RouterLink>
        </div>
        
        <!-- Action buttons on the right side -->
        <div class="headerPart">
          <RouterLink :to="{ name: 'HomeView'}">
            <button class="btn btn-primary border-0 bg-black">
              <i class="bi bi-search"></i>
              <label class="ms-1 label-to-be-hidden">
                Поиск
              </label>
            </button>
          </RouterLink>
          <RouterLink :to="{ name: 'FavouritePhotoesView'}">
            <button class="btn btn-primary border-0 bg-black">
              <i class="bi bi-heart"></i>
              <label class="ms-1 label-to-be-hidden">
                Избранное
              </label>
            </button>
          </RouterLink>
        </div>
      </div>
    </header>
  </template>

<style>
    /* Customize the logo size */
    .logo-img {
        height: 55px; /* Set the maximum height you want for the logo */
        width: auto; /* This ensures the logo scales proportionally */
    }
    .HeaderComponent {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        background-color: #ffffff; /* Adjust this to the background color of your header */
        z-index: 1; /* Ensure the header stays above the background image */
    }
    .headerPart{
        margin: 0 2.5rem 0 2.5rem;
    }
    @media screen and (max-width: 767px) {
        .logo-img {
            height: 35px;
        }
        .headerPart{
            margin: 0 1rem 0 1rem;
        }
        .label-to-be-hidden{
            display: none;
        }
    }
</style>