<template>
  <div class="flex">
    <!-- Backdrop -->
    <div
      :class="isOpen ? 'block' : 'hidden'"
      @click="isOpen = false"
      class="
        fixed
        inset-0
        z-20
        transition-opacity
        bg-black
        opacity-50
        lg:hidden
      "
    ></div>
    <!-- End Backdrop -->

    <div
      :class="isOpen ? 'translate-x-0 ease-out' : '-translate-x-full ease-in'"
      class="
        fixed
        inset-y-0
        left-0
        z-30
        w-64
        overflow-y-auto
        transition
        duration-300
        transform
        bg-gray-900
        lg:translate-x-0 lg:static lg:inset-0
      "
    >
      <div class="flex items-center justify-center mt-8">
        <div class="flex items-center">
          <span class="mx-2 text-2xl font-semibold text-white">PTI</span>
        </div>
      </div>

      <nav class="mt-10">
        <p class="pl-4 text-xs font-semibold mb-4 text-gray-400">Geral</p>
        <router-link
          class="flex items-center px-6 py-2 mt-4 duration-200 border-l-4"
          :class="[$route.name === 'Dashboard' ? activeClass : inactiveClass]"
          to="/dashboard"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
            <path d="M2 10a8 8 0 018-8v8h8a8 8 0 11-16 0z" />
            <path d="M12 2.252A8.014 8.014 0 0117.748 8H12V2.252z" />
          </svg>

          <span class="mx-4">Painel</span>
        </router-link>
        <p class="pl-4 my-2 text-xs font-semibold mb-4 text-gray-400">
          Equipamentos
        </p>
        
        <router-link 
          class="flex items-center px-6 py-2 mt-4 duration-200 border-l-4"
          :class="[$route.name === 'Meters' ? activeClass : inactiveClass]" to="/meters"
        >
          <svg version="1.0" xmlns="http://www.w3.org/2000/svg" width="20" height="20"
            viewBox="0 0 512.000000 512.000000" preserveAspectRatio="xMidYMid meet">
            <g transform="translate(0.000000,512.000000) scale(0.100000,-0.100000)" fill="currentColor" stroke="none">
              <path d="M945 5111 c-101 -26 -187 -97 -231 -194 l-29 -62 -5 -565 -5 -565
            -38 -62 c-127 -208 -229 -504 -274 -793 -23 -157 -24 -462 0 -620 44 -285 148
            -587 274 -793 l38 -62 5 -565 5 -565 27 -57 c35 -76 98 -142 167 -175 l56 -28
            1625 0 1625 0 58 29 c70 36 130 99 165 174 l27 57 5 565 5 565 44 74 c90 153
            172 356 220 547 50 197 65 326 65 544 0 218 -15 347 -65 544 -48 191 -130 394
            -220 547 l-44 74 -5 565 -5 565 -27 57 c-35 76 -98 142 -167 175 l-56 28
            -1605 2 c-883 0 -1618 -2 -1635 -6z m3220 -180 c22 -10 48 -28 58 -39 43 -48
            46 -77 47 -506 l0 -409 -105 111 c-186 195 -389 344 -630 462 -218 106 -407
            166 -660 206 -153 25 -477 25 -630 0 -514 -83 -945 -306 -1289 -667 l-106
            -112 0 409 c1 429 4 458 47 506 10 11 36 29 58 39 38 18 108 19 1605 19 1497
            0 1567 -1 1605 -19z m-1303 -346 c817 -124 1483 -727 1682 -1524 45 -179 60
            -303 60 -501 0 -198 -15 -322 -60 -502 -181 -725 -758 -1301 -1483 -1482 -179
            -45 -303 -60 -501 -60 -410 0 -787 111 -1121 332 -914 602 -1197 1808 -645
            2746 306 520 827 880 1426 986 183 32 453 34 642 5z m-1724 -3726 c315 -262
            683 -427 1107 -496 153 -24 478 -24 630 1 514 83 945 306 1289 667 l106 112 0
            -409 c-1 -429 -4 -458 -47 -506 -10 -11 -36 -29 -58 -39 -38 -18 -108 -19
            -1605 -19 -1497 0 -1567 1 -1605 19 -22 10 -48 28 -58 39 -43 48 -46 77 -47
            506 l0 409 96 -102 c53 -56 140 -138 192 -182z" />
              <path d="M1279 3727 c-81 -54 -93 -107 -87 -366 3 -164 4 -169 31 -210 18 -27
            44 -50 70 -62 41 -18 85 -19 1267 -19 1182 0 1226 1 1267 19 26 12 52 35 70
            62 27 41 28 46 31 210 6 259 -6 312 -87 366 l-34 23 -1247 0 -1247 0 -34 -23z
            m429 -314 l-3 -168 -167 -3 -168 -2 0 170 0 170 170 0 170 0 -2 -167z m512 -3
            l0 -170 -170 0 -170 0 0 170 0 170 170 0 170 0 0 -170z m510 0 l0 -170 -170 0
            -170 0 0 170 0 170 170 0 170 0 0 -170z m510 0 l0 -170 -170 0 -170 0 0 170 0
            170 170 0 170 0 0 -170z m510 0 l0 -170 -167 2 -168 3 -3 168 -2 167 170 0
            170 0 0 -170z" />
              <path d="M922 2619 c-56 -28 -116 -98 -132 -154 -31 -112 35 -423 140 -651
            240 -523 704 -889 1279 -1011 113 -24 148 -27 341 -27 220 -1 285 6 445 46
            684 170 1211 736 1329 1427 28 167 19 232 -46 306 -21 24 -58 53 -81 64 l-43
            21 -1595 0 -1594 0 -43 -21z m3228 -176 c20 -25 21 -36 16 -94 -3 -37 -19
            -119 -35 -183 -154 -610 -627 -1061 -1254 -1198 -88 -19 -132 -22 -317 -22
            -237 0 -314 11 -510 75 -550 180 -970 657 -1081 1230 -24 127 -24 160 2 193
            l20 26 1569 0 1569 0 21 -27z" />
              <path d="M2599 2290 c-8 -5 -97 -132 -197 -282 -201 -302 -207 -315 -151 -362
            30 -26 31 -26 215 -26 101 0 184 -2 184 -5 0 -3 -58 -93 -130 -200 -141 -213
            -149 -233 -104 -279 28 -28 59 -33 96 -15 14 6 98 124 206 285 196 297 204
            313 157 359 -24 25 -26 25 -215 25 -104 0 -190 2 -190 5 0 3 59 93 130 200
            141 213 150 236 105 280 -25 26 -75 33 -106 15z" />
            </g>
          </svg>
          <span class="mx-4">Medidores</span>

        </router-link>

        <div class="absolute bottom-0 left-0 mx-8">
          <img src="../assets/Logo_PTI_Vertical.png" width="200"/>
        </div>
      </nav>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useSidebar } from "../hooks/useSidebar";

const { isOpen } = useSidebar();
const activeClass = ref(
  "bg-gray-600 bg-opacity-25 text-gray-100 border-gray-100"
);
const inactiveClass = ref(
  "border-gray-900 text-gray-500 hover:bg-gray-600 hover:bg-opacity-25 hover:text-gray-100"
);
</script>
