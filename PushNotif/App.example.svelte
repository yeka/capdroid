<script>
	import { PushNotifications } from '@capacitor/push-notifications';

	export let name;

	name = "one notif - push notif";

	let push_state = "";
	let info = "";

	PushNotifications.requestPermissions().then(result => {
      	if (result.receive === 'granted') {
			// Register with Apple / Google to receive push via APNS/FCM
			PushNotifications.register();
			push_state = "ready";
		} else {
			push_state = "error";
    	}
	});

    PushNotifications.addListener('registration', (token) => {
      	info = 'Push registration success, token: ' + token.value;
    });

    PushNotifications.addListener('registrationError', (error) => {
      	info = 'Error on registration: ' + JSON.stringify(error);
    });

    PushNotifications.addListener('pushNotificationReceived', (notification) => {
        info = 'Push received: ' + JSON.stringify(notification);
    });

    PushNotifications.addListener('pushNotificationActionPerformed', (notification) => {
    	info = 'Push action performed: ' + JSON.stringify(notification);
    });
</script>

<main>
	<h1>Welcome to {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<p>Push Notification is {push_state}</p>
	<p>{info}</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>