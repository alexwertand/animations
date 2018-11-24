<template>
	<div id="app">
		<div class="container">
			<div class="row">
				<div class="col-xs-12 col-sm-8 col-md-offset-4 col-md-6 col-md-offset-3">
					<h1>Animations</h1>

					<button class="btn btn-primary" @click="addItem">Add item!</button>
					<br>
					<br>

					<ul class="list-group">
						<transition-group name="slide">
							<li
								style="cursor: pointer"
								class="list-group-item"
								v-for="(number, index) in numbers"
								:key="number"
								@click="removeItem(index)">{{ number }}</li>
						</transition-group>
					</ul>

					<hr>

					<select v-model="alertAnimation" class="form-control">
						<option value="fade">Fade</option>

						<option value="slide">Slide</option>
					</select>

					<br>
					<br>

					<button class="btn btn-primary" @click="show = !show">Show alert!</button>

					<br>
					<br>
					
					<transition :name="alertAnimation">
						<div class="alert alert-info" v-if="show">Some info!</div>
					</transition>

					<transition :name="alertAnimation" type="animation">
						<div class="alert alert-danger" v-if="show">Some info!</div>
					</transition>

					<transition :name="alertAnimation" type="animation" appear>
						<div class="alert alert-info" v-if="show">Some info!</div>
					</transition>

					<transition
							enter-active-class="animated bounce"
							leave-active-class="animated shake"
							type="animation">
						<div class="alert alert-info" v-if="show">Some info! Animate via Animate.css!</div>
					</transition>

					<hr>

					<!-- <transition :name="alertAnimation" mode="out-in">
						<div class="alert alert-info" v-if="show" key="info">Some info!</div>

						<div class="alert alert-danger" v-else key="danger">Some info! Toggler!!</div>
					</transition> -->

					<hr>

					<button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
					<br><br>

					<!-- <transition
						@before-enter="beforeEnter"
						@enter="enter"
						@after-enter="afterEnter"
						@enter-cancelled="enterCancelled"
						
						@before-leave="beforeLeave"
						@leave="leave"
						@after-leave="afterLeave"
						@leave-cancelled="leaveCancelled"
						:css="false">
						<div
								style="width: 100px; height: 100px; background-color: orangered"
								v-if="load"></div>
					</transition> -->

					<br><br>

					<button
							class="btn btn-primary"
							@click="selectedComponent == 'success-alert' ? selectedComponent = 'danger-alert' : selectedComponent = 'success-alert'">Toggle component!</button>

					<br>
					<br>

					<!-- <transition name="fade" mode="out-in">
						<component :is="selectedComponent"></component>
					</transition> -->
					
					<br>
					<br>
				</div>
			</div>
		</div>
	</div>
</template>

<script>

	import DangerAlert from './components/DangerAlert.vue';
	import SuccessAlert from './components/SuccessAlert.vue';

	export default {
		name: 'app',
		data() {
			return {
				show: false,
				load: true,
				alertAnimation: 'fade',
				elementWidth: 100,
				selectedComponent: 'DangerAlert',
				numbers: [1, 2, 3, 4, 5]
			}
		},
		methods: {
			beforeEnter(el) {
				console.log('beforeEnter!');
				this.elementWidth = 100;
				el.style.width = this.elementWidth + 'px';
			},
			enter(el, done) {
				console.log('Enter!');
				let round = 1;
				const interval = setInterval(() => {
					el.style.width = (this.elementWidth + round * 10) + 'px';
					round++;
					if (round > 20) {
						clearInterval(interval);
						done();
					}
				}, 20);
			},
			afterEnter(el) {
				console.log('afterEnter!');
			},
			enterCancelled(el) {
				console.log('enterCancelled!');
			},
			beforeLeave(el) {
				console.log('beforeLeave!');
				this.elementWidth = 300;
				el.style.width = this.elementWidth + "px";
			},
			leave(el, done) {
				console.log('leave!');
				let round = 1;
				const interval = setInterval(() => {
					el.style.width = (this.elementWidth - round * 10) + 'px';
					round++;
					if (round < 1) {
						clearInterval(interval);
						done();
					}
				}, 20);
			},
			afterLeave(el) {
				console.log('afterLeave!');
			},
			leaveCancelled(el) {
				console.log('leaveCancelled!');
			},
			addItem() {
				const pos = Math.floor(Math.random() * this.numbers.length);

				this.numbers.splice(pos, 0, this.numbers.length + 1);
			},
			removeItem(index) {
				this.numbers.splice(index, 1);
			}
		},
		components: {
			DangerAlert, SuccessAlert
		}
	}
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

	.fade-enter {
		opacity: 0;
	}

	.fade-enter-active {
		transition: opacity 1s;
	}

	.fade-leave {

	}

	.fade-leave-active {
		transition: opacity 1s;
		opacity: 0;
	}

	.slide-enter {
		opacity: 0;
	}

	.slide-enter-active {
		animation: slide-in 1s ease-out forwards;
		transition: opacity 1s;
	}

	.slide-leave {
		
	}

	.slide-leave-active {
		animation: slide-out 1s ease-out forwards;
		opacity: 0;
		transition: opacity 0.5s;
		position: absolute;
	}

	.slide-move {
		transition: transform 1s;
	}

	@keyframes slide-in {
		from {
			transform: translateY(20px);
		}

		to {
			transform: translateY(0px);
		}
	}

	@keyframes slide-out {
		from {
			transform: translateY(0px);
		}

		to {
			transform: translateY(20px);
		}
	}
</style>
