<template>
    <div>
        <nav class="flex items-center mt-6 mr-10">
                <img src="~/assets/IntelliSeven_Logo.png" alt="IntelliSeven_Logo" class = "w-50" />
                <ul class="flex-1 text-right">
                    <li class="list-none inline-block px-2"><NuxtLink to = "/" class="hover:underline hover:underline-offset-[30px] decoration-4 decoration-red-600 mr-6"> Home </NuxtLink></li>
                    <li class="list-none inline-block px-2"><NuxtLink to = "/Accounts" class="hover:underline hover:underline-offset-[30px] decoration-4 decoration-red-600 mr-6"> Accounts </NuxtLink></li>
                    <li class="list-none inline-block px-2"><NuxtLink to = "/Sales" class="hover:underline hover:underline-offset-[30px] decoration-4 decoration-red-600 mr-6"> Sales </NuxtLink></li>
                
                <span class="stroke-black-700 w-10 h-10">
                    <li class="list-none inline-block px-2 no-underline hover:underline">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M10 21h4c0 1.1-.9 2-2 2s-2-.9-2-2m11-2v1H3v-1l2-2v-6c0-3.1 2-5.8 5-6.7V4c0-1.1.9-2 2-2s2 .9 2 2v.3c3 .9 5 3.6 5 6.7v6l2 2m-4-8c0-2.8-2.2-5-5-5s-5 2.2-5 5v7h10v-7Z"/></svg>
                    </li>

                    <li class="list-none inline-block px-2 no-underline hover:underline">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="currentColor"><path d="m4 19l-.93-.37a1 1 0 0 0 1.125 1.35L4 19zm4.706-.936l.474-.881l-.317-.17l-.352.07l.195.98zm-3.082-3.147l.93.37l.163-.414l-.196-.399l-.897.443zM19 12c0 3.246-2.853 6-6.53 6v2c4.641 0 8.53-3.514 8.53-8h-2zM5.941 12c0-3.246 2.854-6 6.53-6V4C7.83 4 3.94 7.514 3.94 12h2zm6.53-6C16.147 6 19 8.754 19 12h2c0-4.486-3.889-8-8.53-8v2zm0 12c-1.205 0-2.328-.3-3.291-.817l-.948 1.761A8.934 8.934 0 0 0 12.471 20v-2zm-8.276 1.98l4.706-.936l-.39-1.961l-4.706.936l.39 1.962zm2.326-5.506A5.564 5.564 0 0 1 5.94 12h-2c0 1.2.282 2.338.786 3.36l1.794-.886zm-1.826.073L3.07 18.631l1.858.738l1.624-4.083l-1.858-.739z"/><circle cx="9" cy="12" r="1"/><circle cx="12.5" cy="12" r="1"/><circle cx="16" cy="12" r="1"/></g></svg>
                    </li>

                    <li class="list-none inline-block px-2 no-underline hover:underline">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M12 2.5a5.5 5.5 0 0 1 3.096 10.047a9.005 9.005 0 0 1 5.9 8.181a.75.75 0 1 1-1.499.044a7.5 7.5 0 0 0-14.993 0a.75.75 0 0 1-1.5-.045a9.005 9.005 0 0 1 5.9-8.18A5.5 5.5 0 0 1 12 2.5ZM8 8a4 4 0 1 0 8 0a4 4 0 0 0-8 0Z"/></svg>
                    </li>

                </span>
                </ul>
            
            </nav>
            <hr class="mt-4 border border-gray-300">
        <slot />
    </div>
</template>
<style>
  /* home route and active route will show in bold as it matches / and /about */
  a.router-link-active {
    text-decoration-line: underline;
  }
  /* exact link will show the primary color for only the exact matching link */
  a.router-link-exact-active {
    text-decoration-line: underline;
    text-underline-offset: 30px;
    text-decoration-line: #dc2626;
  }
</style>