<script lang="ts">
	import { PushNotifications } from '@capacitor/push-notifications';
	import { Capacitor } from '@capacitor/core';

	let push_state = "";
	let token = "";
	let info = "";

	if (Capacitor.getPlatform() != 'web') {
		PushNotifications.requestPermissions().then(result => {
			if (result.receive === 'granted') {
				// Register with Apple / Google to receive push via APNS/FCM
				PushNotifications.register();
				push_state = "ready";
			} else {
				push_state = "error";
			}
		});

		PushNotifications.addListener('registration', (t) => {
			token = t.value;
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
	}
</script>

<main>
	<h1>Push Notif Test</h1>
	<p>Registration status: {push_state}</p>
	<p>Token: {token}</p>
	<p>{info}</p>
</main>

<style>
	main {
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>