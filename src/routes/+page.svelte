<script lang="ts">
    const links = ['collections', 'men', 'women', 'about', 'contact'];

    const images = [
        { full: './images/image-product-1.jpg', thumb: './images/image-product-1-thumbnail.jpg' },
        { full: './images/image-product-2.jpg', thumb: './images/image-product-2-thumbnail.jpg' },
        { full: './images/image-product-3.jpg', thumb: './images/image-product-3-thumbnail.jpg' },
        { full: './images/image-product-4.jpg', thumb: './images/image-product-4-thumbnail.jpg' }
    ];

    const price = 125;

    let currentImg = $state(0);
    let lightboxOpen = $state(false);
    let menuOpen = $state(false);
    let quantity = $state(0);
    let cartCount = $state(0);
    let cartOpen = $state(false);

    const total = $derived(price * cartCount);

    function next() {
        currentImg = currentImg+1;
        if(currentImg === images.length){
            currentImg = 0;
        }
    }

    function prev() {
        currentImg = currentImg-1;
        if(currentImg === -1){
            currentImg = images.length -1;
        }
    }

    function decrease() {
        if (quantity > 0) quantity -= 1;
    }

    function increase() {
        quantity += 1;
    }

    function addToCart() {
        cartCount += quantity;
        quantity = 0;
    }

    function openLightbox() {
        if (window.innerWidth >= 768) lightboxOpen = true;
    }

    function handleKeydown(event: KeyboardEvent) {
        if (event.key === 'Escape') {
            lightboxOpen = false;
            menuOpen = false;
        }
        if (!lightboxOpen) return;
        if (event.key === 'ArrowRight') next();
        if (event.key === 'ArrowLeft') prev();
    }
</script>

<svelte:window onkeydown={handleKeydown} />

<div class="mx-auto max-w-6xl md:px-8">
    <header
        class="relative flex h-16 justify-between bg-white px-6
            md:h-24 md:border-b md:border-gray-200 md:px-0"
    >
        <div class="flex items-stretch gap-4 md:gap-10">
            <button
                onclick={() => (menuOpen = true)}
                aria-label="Open menu"
                class="cursor-pointer self-center text-grayish-blue md:hidden"
            >
                <img src="./images/icon-menu.svg" alt="menu">
            </button>

            <img src="./images/logo.svg" alt="logo" class="self-center">

            <nav class="hidden gap-8 tracking-wide text-grayish-blue md:flex">
                {#each links as link}
                    <a
                        href={link}
                        class="-mb-px flex items-center border-b-4 border-transparent capitalize
                            transition-colors hover:border-orange hover:text-very-dark-blue"
                    >
                        {link}
                    </a>
                {/each}
            </nav>
        </div>

        <div class="flex items-center gap-6 md:gap-10">
            <button
                onclick={() => (cartOpen = !cartOpen)}
                aria-label="Toggle cart"
                class="relative cursor-pointer"
            >
                <img src="./images/icon-cart.svg" alt="">
                {#if cartCount > 0}
                    <span
                        class="absolute -top-2 -right-2 rounded-full bg-orange px-2 text-xs font-bold text-white"
                    >
                        {cartCount}
                    </span>
                {/if}
            </button>

            <a href="profile">
                <img
                    src="./images/image-avatar.png"
                    alt="user profile"
                    class="h-6 w-6 rounded-full border-2 border-transparent hover:border-orange
                        md:h-12 md:w-12"
                >
            </a>
        </div>

        {#if cartOpen}
            <div
                class="absolute top-full right-2 left-2 z-40 mt-2 rounded-xl bg-white shadow-2xl
                    md:right-0 md:left-auto md:w-96"
            >
                <h2 class="border-b border-gray-200 p-6 font-bold text-very-dark-blue">Cart</h2>

                <div class="p-6">
                    {#if cartCount === 0}
                        <p class="py-10 text-center font-bold text-grayish-blue">Your cart is empty.</p>
                    {:else}
                        <div class="flex items-center gap-4">
                            <img src={images[0].thumb} alt="" class="h-12 w-12 rounded-md">

                            <div class="flex-1 text-grayish-blue">
                                <p>Fall Limited Edition Sneakers</p>
                                <p>
                                    ${price.toFixed(2)} x {cartCount}
                                    <span class="font-bold text-very-dark-blue">${total.toFixed(2)}</span>
                                </p>
                            </div>

                            <button
                                onclick={() => (cartCount = 0)}
                                aria-label="Remove item from cart"
                                class="cursor-pointer"
                            >
                                <img src="./images/icon-delete.svg" alt="">
                            </button>
                        </div>

                        <button
                            class="mt-6 w-full cursor-pointer rounded-lg bg-orange py-4 font-bold
                                text-very-dark-blue transition-opacity hover:opacity-60"
                        >
                            Checkout
                        </button>
                    {/if}
                </div>
            </div>
        {/if}
    </header>

    <main class="mx-auto md:grid md:max-w-5xl md:grid-cols-2 md:items-center md:gap-24 md:py-20">
        <div class="flex flex-col gap-6">
            <div class="relative">
                <button
                    onclick={openLightbox}
                    class="block w-full overflow-hidden md:cursor-pointer md:rounded-2xl"
                >
                    <img
                        src={images[currentImg].full}
                        alt="Sneaker product photo {currentImg + 1}"
                        class="h-75 w-full object-cover md:aspect-square md:h-auto"
                    >
                </button>

                <button
                    onclick={prev}
                    aria-label="Previous image"
                    class="absolute top-1/2 left-4 flex h-10 w-10 -translate-y-1/2 cursor-pointer
                        items-center justify-center rounded-full bg-white text-very-dark-blue
                        transition-colors hover:text-orange md:hidden"
                >
                    <img src="./images/icon-previous.svg" alt="previous">
                </button>

                <button
                    onclick={next}
                    aria-label="Next image"
                    class="absolute top-1/2 right-4 flex h-10 w-10 -translate-y-1/2 cursor-pointer
                        items-center justify-center rounded-full bg-white text-very-dark-blue
                        transition-colors hover:text-orange md:hidden"
                >
                    <img src="./images/icon-next.svg" alt="next">
                </button>
            </div>

            <div class="hidden grid-cols-4 gap-4 md:grid">
                {#each images as image, index}
                    <button
                        onclick={() => (currentImg = index)}
                        class="cursor-pointer overflow-hidden rounded-xl border-2
                            {index === currentImg ? 'border-orange' : 'border-transparent'}"
                    >
                        <img
                            src={image.thumb}
                            alt="Sneaker thumbnail {index + 1}"
                            class="aspect-square w-full object-cover
                                {index === currentImg ? 'opacity-50' : 'hover:opacity-70'}"
                        >
                    </button>
                {/each}
            </div>
        </div>

        <div class="flex flex-col gap-4 px-6 pt-6 pb-12 md:gap-6 md:p-0">
            <p class="text-xs font-bold tracking-widest text-grayish-blue uppercase md:text-sm">
                Sneaker Company
            </p>
            <h1 class="text-3xl leading-tight font-bold text-very-dark-blue md:text-5xl">
                Fall Limited Edition Sneakers
            </h1>
            <p class="leading-relaxed text-grayish-blue">
                These low-profile sneakers are your perfect casual wear companion. Featuring a durable
                rubber outer sole, they’ll withstand everything the weather can offer.
            </p>

            <div class="flex items-center justify-between md:block">
                <div class="flex items-center gap-4">
                    <span class="text-3xl font-bold text-very-dark-blue">${price.toFixed(2)}</span>
                    <span class="rounded-md bg-very-dark-blue px-2 py-0.5 font-bold text-white">50%</span>
                </div>
                <p class="font-bold text-grayish-blue line-through md:mt-2">$250.00</p>
            </div>

            <div class="flex flex-col gap-4 md:flex-row">
                <div
                    class="flex items-center justify-between rounded-lg bg-gray-100 px-6 py-4
                        md:w-40 md:px-4"
                >
                    <button onclick={decrease} aria-label="Decrease quantity" class="cursor-pointer">
                        <img src="./images/icon-minus.svg" alt="">
                    </button>
                    <span class="font-bold text-very-dark-blue">{quantity}</span>
                    <button onclick={increase} aria-label="Increase quantity" class="cursor-pointer">
                        <img src="./images/icon-plus.svg" alt="">
                    </button>
                </div>

                <button
                    onclick={addToCart}
                    class="flex cursor-pointer items-center justify-center gap-4 rounded-lg bg-orange
                        py-4 font-bold text-very-dark-blue shadow-lg shadow-orange/40
                        transition-opacity hover:opacity-60 md:flex-1"
                >
                    <img src="./images/icon-cart.svg" alt="" class="brightness-0">
                    Add to cart
                </button>
            </div>
        </div>
    </main>
</div>

{#if menuOpen}
    <div
        class="fixed inset-0 z-50 bg-black/75 md:hidden"
        role="dialog"
        aria-modal="true"
        aria-label="Site menu"
    >
        <div class="h-full w-2/3 max-w-xs bg-white p-6">
            <button
                onclick={() => (menuOpen = false)}
                aria-label="Close menu"
                class="cursor-pointer text-grayish-blue transition-colors hover:text-very-dark-blue"
            >
                <svg width="15" height="15" viewBox="0 0 15 15" fill="none"
                    stroke="currentColor" stroke-width="3" aria-hidden="true">
                    <path d="M2 2l11 11M13 2L2 13" />
                </svg>
            </button>

            <nav class="mt-12 flex flex-col gap-5 text-lg font-bold text-very-dark-blue">
                {#each links as link}
                    <a href={link} class="capitalize">{link}</a>
                {/each}
            </nav>
        </div>
    </div>
{/if}

{#if lightboxOpen}
    <div
        class="fixed inset-0 z-50 hidden items-center justify-center bg-black/75 md:flex"
        role="dialog"
        aria-modal="true"
        aria-label="Product image gallery"
    >
        <div class="relative w-full max-w-lg">
            <button
                onclick={() => (lightboxOpen = false)}
                aria-label="Close gallery"
                class="absolute -top-10 right-0 cursor-pointer text-white transition-colors hover:text-orange"
            >
                <svg width="16" height="16" viewBox="0 0 16 16" fill="none"
                    stroke="currentColor" stroke-width="3" aria-hidden="true">
                    <path d="M2 2l12 12M14 2L2 14" />
                </svg>
            </button>

            <div class="relative">
                <img
                    src={images[currentImg].full}
                    alt="Sneaker product photo {currentImg + 1}"
                    class="aspect-square w-full rounded-2xl object-cover"
                >

                <button
                    onclick={prev}
                    aria-label="Previous image"
                    class="absolute top-1/2 -left-7 flex h-14 w-14 -translate-y-1/2 cursor-pointer
                        items-center justify-center rounded-full bg-white text-very-dark-blue
                        transition-colors hover:text-orange"
                >
                    <svg width="12" height="18" viewBox="0 0 12 18" fill="none"
                        stroke="currentColor" stroke-width="3" aria-hidden="true">
                        <path d="M11 1 3 9l8 8" />
                    </svg>
                </button>

                <button
                    onclick={next}
                    aria-label="Next image"
                    class="absolute top-1/2 -right-7 flex h-14 w-14 -translate-y-1/2 cursor-pointer
                        items-center justify-center rounded-full bg-white text-very-dark-blue
                        transition-colors hover:text-orange"
                >
                    <svg width="12" height="18" viewBox="0 0 12 18" fill="none"
                        stroke="currentColor" stroke-width="3" aria-hidden="true">
                        <path d="m2 1 8 8-8 8" />
                    </svg>
                </button>
            </div>

            <div class="mx-auto mt-10 grid w-4/5 grid-cols-4 gap-6">
                {#each images as image, index}
                    <button
                        onclick={() => (currentImg = index)}
                        class="cursor-pointer overflow-hidden rounded-xl border-2 bg-white
                            {index === currentImg ? 'border-orange' : 'border-transparent'}"
                    >
                        <img
                            src={image.thumb}
                            alt="Sneaker thumbnail {index + 1}"
                            class="aspect-square w-full object-cover
                                {index === currentImg ? 'opacity-50' : 'hover:opacity-70'}"
                        >
                    </button>
                {/each}
            </div>
        </div>
    </div>
{/if}