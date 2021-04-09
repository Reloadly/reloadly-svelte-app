<script>
	const accessToken = __myapp.env.ACCESS_TOKEN

	let results;
	let operatorId = '341'
	let recipientPhone = {
		'countryCode' : 'NG',
		'number' : ''
	}
	let amount = ''
	let customIdentifier = ''
	
	const hold = ( customIdentifier ) => {
		return customIdentifier == ''
	  }
	
	let headers = {
		'Content-Type' : 'application/json',
		'Authorization' :  accessToken 
	}
	
	const doPost = async () => {
		const response = await fetch('https://topups.reloadly.com/topups', {
			method: 'POST',
			body: JSON.stringify({
				recipientPhone,
				amount,
				operatorId,
				customIdentifier
			}),
			headers: headers
		})
		if (response.status === 200) {
      return await response.json();
    } else {
      throw new Error(response.statusText);
    }	
	}

	const handleClick = () => {
    results = doPost();
  }
</script>

<div>

  <div class = 'request'>
	<h1>RECHARGE ON THE GO  &#127757;</h1>
	<p> Phone number:   <input type=number bind:value={recipientPhone.number} max=5 /> </p>
	<p> Amount (NGN): <input type=number bind:value={amount} /> </p>
	<p> Transaction Reference: <input bind:value={customIdentifier} /> </p>
	<button disabled='{hold(customIdentifier)}'   on:click={handleClick}>
	  RECHARGE
	</button>
  </div>

  <div class = 'response'>
	{#await results }
		<p>Loading...</p>
	{:then res}
		<p>{res ? `Transaction ID: ${res.operatorTransactionId}` : ''}</p>
		<p>{res ? `Custom Identifier: ${res.customIdentifier}` : ''}</p>
		<p>{res ? `Operator Name: ${res.operatorName}` : ''}</p>
		<p>{res ? `Trannsaction Date: ${res.transactionDate}` : ''}</p>
	
	{:catch res}
		<p>Error message: {res ? `${res.message}` : ''}</p>
	{/await}
  </div>
</div>
	
<style>
		
  .request {
	padding: 20px;
	background-color: #242D3D;
	color: #1EBAD5;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	align-items: center;
	text-align: center;
  }
		
  .response {
	padding: 20px;
	background-color: #1EBAD5;
	color: #242D3D;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	align-items: center;
	text-align: center;
  }
		
  button {
	background-color: #1EBAD5;
	color: #242D3D;
	border-radius: 30px;
	padding: 15px;
	border: none
  }		
</style>