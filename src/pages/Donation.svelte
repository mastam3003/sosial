<script>
	import { charity, getCharity } from "../stores/data.js";
	import { params } from "../stores/pages.js";
	import router from 'page';
	import Header from '../components/Header.svelte';
    import Footer from '../components/Footer.svelte';
	import Loader from '../components/Loader.svelte';

	let  amount = 0, 
	name, 
	email, 
	agree = false,
	contribute = 0 ;

	$: if($charity) {
		contribute = Math.floor((parseInt(amount) / $charity.target) *100);
	}

	getCharity($params.id);



	function handleButtonClick() {
		console.log("Button click");
	}

	async function handleForm(event) {
		agree = false;                                                                                                                                                                                    
		const newData = await getCharity ($params.id);
		newData.pledged = newData.pledged + parseInt (amount);
		try {
			const res = await fetch(`https://charity-api-bwa.herokuapp.com/charities/${$params.id}`,
		{
		  method: 'PUT',
		  headers: {
			  "content-type": "application/json",
		  },
		  body: JSON.stringify(newData),
		}
	);
	const resMid = await fetch('/.netlify/functions/payment', {
		method: "POST",
		headers: {
			"content-type": "application/json",
		},
		body: JSON.stringify({
			id: $params.id,
			amaount: parseInt(amount),
			name,
			email,
		}),
	});
	const midtransData = await resMid.json();
	console.log(midtransData);
	windows.location.href = midtransData.url;
	} catch(err) {
		console.log(err);
	}
	}
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
    .xs-donation-form-images{
        text-align: center;
    }
</style>

<Header />

<!-- welcome section -->
	<!--breadcumb start here-->
	{#if !$charity}
	<Loader />
	{:else }
	<section class="xs-banner-inner-section parallax-window" 
	style="background-image:url('/assets/images/bantu.jpg')">
	<div class="xs-black-overlay"></div>
	<div class="container">
	<div class="color-white xs-inner-banner-content">
	<h2>Halaman Donasi</h2>
	<p>{$charity.title}</p>
	<ul class="xs-breadcumb">
	<li class="badge badge-pill badge-primary">
	<a href="/" class="color-white">Home /</a> Donate
	</li>
	</ul>
	</div>
	</div>
	</section><!--breadcumb end here--><!-- End welcome section -->
	<main class="xs-main">
	<!-- donation form section -->
	<section class="xs-section-padding bg-gray">
	<div class="container">
	<div class="row">
	<div class="col-lg-6">
	<div class="xs-donation-form-images"><img src={$charity.thumbnail} class="img-responsive" alt=""></div>
	</div>
	<div class="col-lg-6">
	<div class="xs-donation-form-wraper">
	<div class="xs-heading xs-mb-30">
	<h2 class="xs-title">{$charity.title}</h2>
	<p class="small">To learn more about make donate charity
	with us visit our "<span class="color-green">Contact
	us</span>" site. By calling <span class=
	"color-green">+62 857 0213 4368</span>.</p><h5>Youre donation will be contributing <strong>{contribute}%</strong>of total current donation.</h5><span class=
	"xs-separetor v2"></span>
	</div><!-- .xs-heading end -->
	<form 
	on:submit|preventDefault={handleForm}
	action="#"
	method="post"
	id="xs-donation-form" 
	class="xs-donation-form" 
	name="xs-donation-form">
	<div class="xs-input-group">
	<label for="xs-donate-name">
		Besaran Donasi 
		<span class="color-light-red">**</span>
	</label> 
	<input 
		type="text" 
		name="amount" 
		id="xs-donate-amount" 
		class="form-control" 
		bind:value={amount}
		required="true"
		placeholder="Your donation in Rupiah" />
</div>
<!-- .xs-input-group END -->
<div class="xs-input-group">
	<label for="xs-doante-name">
		Nama 
		<span class="color-light-red">**</span>
	</label>
	<input
		type="text"
		name="name"
		id="xs-doante-name"
		class="form-control"
		bind:value={name}
		required="true"
		placeholder="Your awesome name" />
</div>
<div class="xs-input-group">
	<label for="xs-donate-email">
		Email
		<span class="color-light-red">**</span>
	</label>
	<input
		type="email"
		name="email"
		required="true"
		bind:value={email}
		id="xs-donate-email"
		class="form-control"
		placeholder="email@awesome.com" />
</div>
<div class="xs-input-group" id ="xs-input-checkbox">
	<input type="checkbox" name="agree" id="xs-donate-agree" 
	bind:checked={agree}/>
	<label for="xs-donate-agree"> 
		Siap berdonasi
		<span class="color-light-red">**</span>
	</label>
</div>
<!-- .xs-input-group END -->
<button 
	type="submit"
	disabled={!agree}
	class="btn btn-warning">
	<span class="badge">
	<i class="fa fa-heart" />
	</span> 
	Donate now
	</button>
	</form><!-- .xs-donation-form #xs-donation-form END -->
	</div>
	</div>
	</div><!-- .row end -->
	</div><!-- .container end -->
	</section><!-- End donation form section -->
	</main>
	{/if}



<Footer />