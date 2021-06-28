<template>
	<div>
		<div v-if="isAdminView">
			<h1>AdminView</h1>
			<video id="localVideo" :srcObject.prop ="localStream" playsinline autoplay muted></video>
			<div class="box">
				<button id="startButton" @click="start" :disabled = "isStarted">Start</button>
				<button id="hangupButton" @click="hangup" :disabled = "!isStarted">Hang Up</button>
			</div>
		</div>
		<video v-else id="remoteVideo" :srcObject.prop ="localStream" playsinline autoplay></video>
	</div>
</template>

<script>

export default {
	name : 'LiveVideo',
	props: [
		'isAdminView'
	],
	data(){
		return{
			isStarted : false,
			localStream : MediaStream,
			startTime: 0
		};
	},
	
	methods:{
		start : async function() {
			console.log('Requesting local stream');
			this.isStarted = true;
			try {
				const stream = await navigator.mediaDevices.getUserMedia({audio: true, video: true});
				console.log('Received local stream' + stream);
				this.localStream = stream;
			} catch (e) {
				alert(`getUserMedia() error: ${e.name}`);
			}
		},

		// call : async function() {
		// 	this.isCallMade = true;
		// 	console.log('Starting call');
		// 	this.startTime = window.performance.now();
			
		// 	const configuration = {};
		// 	console.log('RTCPeerConnection configuration:', configuration);
		// 	this.pc1 = new RTCPeerConnection(configuration);
		// 	console.log('Created local peer connection object pc1');
		// 	this.pc1.addEventListener('icecandidate', e => this.onIceCandidate(this.pc1, e));
		// 	this.pc2 = new RTCPeerConnection(configuration);
		// 	console.log('Created remote peer connection object pc2');
		// 	this.pc2.addEventListener('icecandidate', e => this.onIceCandidate(this.pc2, e));
		// 	this.pc1.addEventListener('iceconnectionstatechange', e => this.onIceStateChange(this.pc1, e));
		// 	this.pc2.addEventListener('iceconnectionstatechange', e => this.onIceStateChange(this.pc2, e));
		// 	this.pc2.addEventListener('track', this.gotRemoteStream);

		// 	this.localStream.getTracks().forEach(track => this.pc1.addTrack(track, this.localStream));
		// 	console.log('Added local stream to pc1');

		// 	try {
		// 		console.log('pc1 createOffer start');
		// 		const offer = await this.pc1.createOffer(this.offerOptions);
		// 		await this.onCreateOfferSuccess(offer);
		// 	} catch (e) {
		// 		console.log('Received local stream');
		// 		throw e;
		// 	}
		// },

		// onCreateOfferSuccess : async function(desc) {
		// 	console.log(`Offer from pc1\n${desc.sdp}`);
		// 	console.log('pc1 setLocalDescription start');
		// 	try {
		// 		await this.pc1.setLocalDescription(desc);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e
		// 	}

		// 	console.log('pc2 setRemoteDescription start');
		// 	try {
		// 		await this.pc2.setRemoteDescription(desc);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e
		// 	}

		// 	console.log('pc2 createAnswer start');
		// 	try {
		// 		const answer = await this.pc1.createAnswer();
		// 		await this.onCreateAnswerSuccess(answer);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e;
		// 	}
		// },

		// gotRemoteStream : function(e) {
		// 	if (this.remoteVideo.srcObject !== e.streams[0]) {
		// 		this.remoteVideo.srcObject = e.streams[0];
		// 		console.log('pc2 received remote stream');
		// 	}
		// },

		// onCreateAnswerSuccess : async function(desc) {
		// 	try {
		// 		await this.pc1.setLocalDescription(desc);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e;
		// 	}
		// 	try {
		// 		await this.pc1.setRemoteDescription(desc);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e;
		// 	}
		// },

		// onIceCandidate : async function(pc, event) {
		// 	try {
		// 		await (this.getOtherPc(pc).addIceCandidate(event.candidate));
		// 		console.log(pc);
		// 	} catch (e) {
		// 		console.log('pc1 setLocalDescription start');
		// 		throw e;
		// 	}
		// },

		// onIceStateChange : function(pc, event) {
		// 	if (pc) {
		// 		console.log(`${this.getName(pc)} ICE state: ${pc.iceConnectionState}`);
		// 		console.log('ICE state change event: ', event);
		// 	}
		// },

		hangup : function() {
			console.log('Ending call');
			this.localStream = null;
			this.isStarted=false;
		}

	}
};
</script>

<style scoped>
	button {
	margin: 0 20px 0 0;
	width: 83px;
	}

	button#hangupButton {
		margin: 0;
	}

	video {
		--width: 45%;
		width: var(--width);
		height: calc(var(--width) * 0.75);
		margin: 0 0 20px 0;
		vertical-align: top;
	}

	video#localVideo {
	margin: 0 20px 20px 0;
	}

	div.box {
	margin: 1em;
	}

	@media screen and (max-width: 400px) {
		button {
			width: 83px;
			margin: 0 11px 10px 0;
		}

		video {
			height: 90px;
			margin: 0 0 10px 0;
			width: calc(50% - 7px);
		}
		video#localVideo {
			margin: 0 10px 20px 0;
		}

	}
</style>
