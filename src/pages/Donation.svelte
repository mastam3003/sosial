<script>
    import { onMount } from "svelte";
    import Header from "../components/Header.svelte";
    import Footer from "../components/Footer.svelte";

    export let params;
    let charity;

    async function getCharity(id) {
        const res = await fetch ('https://charity-api-bwa.herokuapp.com/charities/${id}')
        return res.json();
    }

    onMount(async function() {
        charity = await getCharity(params.id)
    });
</script>

<style>
    #xs-input-checkbox {
        display: flex;
        align-items: center;
    }
    #xs-donate-agree {
        width: 40px;
        margin-right: 10px;
    }
    label [for="xs-donate-agree"] {
        margin: 0;
        margin-left: 20px;
    }
    .xs-donation-form-images{
        text-align: center;
    }
</style>

<Header />
<!-- welcome section -->
<!--breadcumb start here-->
{#if charity}
<section 
    class="xs-banner-inner-section parallax-window" 
    style="background-image:url('/assets/images/christian-dubovan-Y_x747Yshlw-unsplash.jpg')">
	<div class="xs-black-overlay" />
	<div class="container">
	    <div class="color-white xs-inner-banner-content">
	        <h2>Donate Now</h2>
	        <p>{charity.title}</p>
	        <ul class="xs-breadcumb">
	            <li class="badge badge-pill badge-primary">
	              <a href="/" class="color-white">Home /</a> 
                    Donate
	            </li>
	        </ul>
	    </div>
	</div>
	</section>
	<!--breadcumb end here-->
    <!-- End welcome section -->
	<main class="xs-main">
        <!-- donation form section -->
	    <section class="xs-section-padding bg-gray">
	        <div class="container">
	            <div class="row">
	            <div class="col-lg-6">
	                <div class="xs-donation-form-images">
                        <img 
                            src="{charity.thumbanail}" 
                            class="img-responsive" 
                            alt="Family Images">
                    </div>
	            </div>
	            <div class="col-lg-6">
	                <div class="xs-donation-form-wraper">
	                    <div class="xs-heading xs-mb-30">
	                        <h2 class="xs-title">{charity.title}</h2>
	                        <p class="small">
                                To learn more about make donate charity with us visit our "
                                <span class="color-green">Contactus</span>
                                " site. By calling 
                                <span class="color-green">+44(0) 800 883 8450</span>
                                .       
                            </p>
                            <span class= "xs-separetor v2"/>
	                    </div>
	                    <!-- .xs-heading end -->
	                    <form 
                            action="#" 
                            method="post" 
                            id="xs-donation-form" 
                            class= "xs-donation-form" 
                            name="xs-donation-form">
	                        <div class="xs-input-group">
	                            <label for="xs-donate-name">
                                    Donation Amount 
                                    <span class="color-light-red">**</span>
                                </label> 
                                <input 
                                    type="text" 
                                    name="amount" 
                                    id="xs-donate-name" 
                                    class="form-control" 
                                    placeholder="Minimum of $5" />
	                        </div>
	                        <!-- .xs-input-group END -->
                            <div class="xs-input-group">
                                <label for="xs-doante-name">
                                    Your Name
                                    <span class="color-light-red">**</span>
                                </label>
                                <input
                                    type="text"
                                    name="name"
                                    id="xs-doante-name"
                                    class="form-control"
                                    placeholder="Your awesome name" />
                            </div>
                            <div class="xs-input-group">
                                <label for="xs-donate-email">
                                    Your Email
                                    <span class="color-light-red">**</span>
                                </label>
                                <input
                                    type="email"
                                    name="email"
                                    id="xs-donate-email"
                                    class="form-control"
                                    placeholder="email@awesome.com" />
                            </div>
                            <div class="xs-input-group" id ="xs-input-checkbox">
                                <input type="checkbox" name="agree" id="xs-donate-agree" />
                                <label for="xs-donate-agree"> 
                                    I Agree
                                    <span class="color-light-red">**</span>
                                </label>
                            </div>
	                        <!-- .xs-input-group END -->
	                        <button type="submit" class="btn btn-warning">
                                <span class="badge">
                                <i class="fa fa-heart" />
                                </span> 
                                Donate now
                            </button>
	                    </form>
	                    <!-- .xs-donation-form #xs-donation-form END -->
	                </div>
	            </div>
	        </div>
            <!-- .row end -->
	    </div>
          <!-- .container end -->
	    </section>
        <!-- End donation form section -->
	</main>
{/if}

<Footer /> 