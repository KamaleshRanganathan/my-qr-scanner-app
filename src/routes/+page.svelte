<script>
    import Footer from '../Components/Footer.svelte';
    import Scanner from '../Components/Scanner.svelte';

    import { onMount } from 'svelte';
    import imagedark from '../images/dark.png';
    import imagelight from '../images/light.png';
    import toggleon from '../images/toggle-on.png';
    import toggleoff from '../images/toggle-off.png';

    let toggle = false;
    let imgUrl = imagedark; 
    let toggleUrl = toggleoff;
    // export let primarycol = 'bg-white';
    // export let secondarycol = 'text-gray-800';
    export let primarycol = 'bg-[#FCF6F5]';
    export let secondarycol ='text-[#2BAE66]';

    function toggleMode() { 
        toggle = !toggle; 
        imgUrl = toggle ? imagelight : imagedark;
        primarycol = toggle ? 'bg-[#101820]' : 'bg-[#FCF6F5]'; 
        toggleUrl = toggle ? toggleon : toggleoff;  

    }

    onMount(() => {
        // Apply initial styles based on system preference
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            toggleMode(); 
        }
    });
</script>

<main class=" {primarycol} min-h-screen">
    <header class="fixed top-0 left-0 right-0 border {toggle ? 'border-white' : 'border-slate-900'}">
               
        <header class="{primarycol} {secondarycol} p-5 flex justify-between items-center">
        
            <div class="flex items-center">
                <img src="{imgUrl}" alt="Logo" class="h-10 mr-3"> 
                <span class="text-xl font-bold">Quick QR Scanner</span> 
            </div>
        
            <nav class="space-x-4 mx-6 flex items-center">
                <button on:click={toggleMode} class="flex items-center"> 
                    <img src="{toggleUrl}" alt="Logo" class="h-10 mr-3"> 
                </button>
                <a href="#contact" class="hover:text-blue-400 font-extrabold tracking-wide">Contact Us</a>
                <a href="#about" class="hover:text-blue-400 font-extrabold tracking-wide"><span>About Us</span></a>
            </nav>
        
        </header>
    </header>
    <Scanner primarycol={primarycol} secondarycol={secondarycol} toggle={toggle} />
    <Footer primarycol={primarycol} secondarycol={secondarycol} toggle={toggle}/>
</main>