<script>
    import { onMount } from 'svelte'
    import { goto } from '$app/navigation';
    
    export let data
    let count = 5
    let unsplashRef = "?utm_source=sparks&utm_medium=referral"

    let photos = []
    let photo = {}
    let photoId = 0

    let paused = false
    export let photoTimer
    let timer = photoTimer

    let description = false
    let started = false

    $: photos = data.photos
    $: photo = photos[photoId]

    function getPhotos() {
        
    }

    function updatePhotos() {
            const update = setInterval(() => {
            if(photoId < photos.length) {
                if(!paused) {
                    if(timer < 1) {
                        nextPhoto()
                    } else {
                        timer = timer - 1 
                    }
                }
            } else {
                clearInterval(update)
            }
        }, 1000)
    }

    function nextPhoto() {
        photoId += 1
        photo = photos[photoId]
        timer = photoTimer
    } 

    function goHome() {
        goto('/')
    }

    onMount(() => {
        updatePhotos()
    })

</script>

<main>
    
    <div class="fixed top-0 left-0 w-full h-full bg-zinc-800">
    {#if photoId < photos.length}
        <img src={photo.urls.raw} alt="" class="h-screen w-fit mx-auto">

        {#if paused}
            <svg class="fixed inset-0 m-auto w-48 h-48 stroke-zinc-100 opacity-75" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
            </svg>
        {/if}
    {:else}
        <div class="w-full h-full m-auto flex flex-col items-center place-items-center justify-center">
            <h1 class="text-3xl font-bold p-4 text-white"> Alle billeder vist </h1>
            <button on:click={goHome} class="text-center py-4 px-12 bg-zinc-900 rounded drop-shadow-md text-white text-xl font-bold"> Tilbage til startsiden </button>
        </div>
            
    {/if}
    </div>

    <div class="fixed bottom-0 left-0 flex flex-row gap-4 m-4">
        <p class="text-2xl font-bold text-zinc-200"> {timer} </p>
    </div>

    <div class="fixed inset-x-0 top-0 flex flex-row w-full justify-between items-center">
        <div class="m-4 text-center">
            <p class="text-2xl font-bold text-zinc-200"><strong>{photoId+1}</strong> / <span class="text-zinc-400">{photos.length}</span></p>
        </div>
        <button class="w-12 h-12 m-4 stroke-zinc-200 cursor-pointer" on:click={goHome}>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="white">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
        </button>
    </div>

    {#if description}
        <div class="fixed bottom-0 right-0 mb-24 mr-4 py-8 px-16 bg-zinc-900 rounded" id="triangleTgt">
            <p class="text-xl text-zinc-200"> {photo.alt_description ? photo.alt_description : "n/a"}</p>
            <div class="flex flex-col">
                <div class="flex flex-row justify-between">
                    <a class="p-2 flex flex-row gap-2 text-white self-start" href={photo.user.links.html + unsplashRef} target="_blank">
                        <img src={photo.user.profile_image.small} class="w-6 h-6 rounded-full" alt="photographers unsplash profile pic"/>
                        <span> {photo.user.name ? photo.user.name : "n/a"} </span>
                    </a>
                    <div class="p-2 flex flex-row gap-2 text-white self-end">
                        <span>
                            {photo.location.name ? photo.location.name : (photo.user.location ? photo.user.location : "n/a")}
                        </span>
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M15 10.5a3 3 0 11-6 0 3 3 0 016 0z" />
                            <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 10.5c0 7.142-7.5 11.25-7.5 11.25S4.5 17.642 4.5 10.5a7.5 7.5 0 1115 0z" />
                        </svg>
                    </div>
                </div>
                <a class="p-2 text-white flex flex-row gap-2 self-end" href={photo.links.html + unsplashRef} target="_blank"> 
                      <span>Open on Unsplash </span>
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 stroke-white">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25" />
                      </svg>
                </a>
            </div>
            
        </div>
    {/if}

    <div class="fixed bottom-0 right-0 flex flex-row gap-4 m-4">

        <svg on:click={nextPhoto} class="w-12 h-12 stroke-zinc-200 cursor-pointer" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M3 8.688c0-.864.933-1.405 1.683-.977l7.108 4.062a1.125 1.125 0 010 1.953l-7.108 4.062A1.125 1.125 0 013 16.81V8.688zM12.75 8.688c0-.864.933-1.405 1.683-.977l7.108 4.062a1.125 1.125 0 010 1.953l-7.108 4.062a1.125 1.125 0 01-1.683-.977V8.688z" />
          </svg>
          
        <svg on:click={() => paused = !paused} class="w-12 h-12 stroke-zinc-200 cursor-pointer" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M14.25 9v6m-4.5 0V9M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
            
        <svg on:click={() => description = !description} class="w-12 h-12 stroke-zinc-200 cursor-pointer" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
        </svg> 
            
            
    </div>
</main>

<style lang="postcss">
    #triangleTgt::after {
        content: "";
        width: 25px;
        height: 25px;
        margin: 100% auto;
        top: 100%;
        right: 50%;
        position: absolute;
        margin-top: -12.5px;
        margin-right: -48%;
        transform: rotate(45deg);
        border-top: 0;
        border-left: 0;
        background: #18181b;
    }

</style>