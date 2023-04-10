<template>
	<div>		
		<div v-if="available === false">
			<div class="setup-container d-flex flex-direction-col justify-between align-center main-bg" v-if="!isStarted">
				<div class="setup">
					<Logo/>
					<div class="hero-banner d-flex">
						<img src="../assets/images/sky_logo.svg" alt="">
					</div>
					<div class="description">
						<p>
							<strong>Glamour blesti kot številka <span>1</span> za<br class="sp"> vedno.</strong> 
							Za to, da tako močno sije, pa se <br class="sp">moramo <strong>zahvaliti predvsem tebi.</strong>
						</p> 
						<p>
							Veseli smo, da znaš prepoznati <strong>sijajne<br class="sp"> lastnosti glamurozne znamke.</strong> 
							Zato te<br class="sp"> vabimo, da <strong>svoje znanje o njej <br class="pc"> osvežiš<br class="sp"> v aktivaciji</strong> in <strong>ujameš priložnost za<br class="sp"> ekskluzivno druženje.</strong>
						</p>					
						<p>
							<strong>Čaka te zabava nad mestom na<br class="sp"> najbolj GLAM lokaciji v Ljubljani.</strong>
						</p>
					</div>
					<div class="nebo-img-wrapper">
						<img src="../assets/images/nebo_large.svg" alt="">
					</div>
					<p>
						<strong>Hitro sodeluj</strong> v aktivaciji, saj je število<br class="sp"> mest na dogodku <strong>omejeno.</strong><br> 
						Tvoj pa bo tudi nov <strong>unikaten žig.</strong>
					</p>
					<div class="action-wrapper">
						<button type="button" class="btn primary" @click="startQuiz()">NA AKTIVACIJO</button>
					</div>				
				</div>
			</div>
			<div v-if="isStarted">
				<div class="quiz animate-fadeIn" v-if="currentQuestionIndex < questions.length && chosenAnswers[currentQuestionIndex] == undefined " :key="currentQuestionIndex">
					<Logo/>
					<div class="hero-banner d-flex">
						<img src="../assets/images/sky_logo.svg" alt="">
					</div>
					<h3 class="subtitle" v-if="currentQuestionIndex == 0">
						Pred tabo je trditev <strong>o najbolj<br class="sp">
						GLAM znamki.</strong><br>
						Pokaži svoje znanje o znamki Glamour in<br class="sp">
						označi, ali zastavljena trditev o njej<br>
						<strong>DRŽI ali NE DRŽI.</strong>
					</h3>
					<div class="answers">
						<p class="overview" v-html="questions[currentQuestionIndex].description"></p>
						<div class="answers--container">
							<div class="answer answer--option" v-for="(answer, index) in questions[currentQuestionIndex].answers" @click="selectAnswer(index)">{{ answer }}</div>						
						</div>
					</div>
				</div>
				<div class="result animate-right" v-else-if="chosenAnswers[currentQuestionIndex] != undefined">	
					<Logo/>			
					<div class="hero-banner text-center">
						<img src="../assets/images/sky_logo.svg" alt="Quiz">
					</div>				
					<h2 class="title" v-html="currentQuestionResult.title"></h2>
					<div class="product">
						<p class="product--description" v-html="currentQuestionResult.description"></p>
						<div class="product--info" v-if="questions[currentQuestionIndex].product_info != undefined">
							<img :src="require(`../assets/images/${questions[currentQuestionIndex]['product_info'].image_path}`)" alt="thumbnail">
							<div class="detail">
								<h3 class="brand">Glamour</h3>
								<h4 class="name">{{ questions[currentQuestionIndex]['product_info'].name }}</h4>
								<div class="tar">
									<h5 class="title">KATRAN</h5>
									<p><span class="number">{{ questions[currentQuestionIndex]['product_info'].tar_amount }}</span> mg</p>
								</div>
								<div class="price">{{ `${questions[currentQuestionIndex]['product_info'].price} €` }}</div>
							</div>
						</div>
					</div>
					<div class="action-wrapper">
						<button class="btn primary" @click="nextQuestion()">NAPREJ</button>
					</div>
				</div>
				<div class="select-entry-station animate-fadeIn" v-else-if="currentQuestionIndex >= questions.length && confirmed == 0">
					<Logo/>
					<div class="top">
						<div class="hero-banner d-flex">
							<img src="../assets/images/sky_logo.svg" alt="">						
						</div>
						<div class="description text-center">
							<p>
								Glamurozno. Znova ti je uspelo dokazati,<br class="sp"> 
								da <strong>tvoje znanje o Glamour blagovni<br> 
								znamki zares blesti.</strong> Z bliskovito hitrim<br class="sp">
								sodelovanjem v aktivaciji si ujel/-a<br>
								<strong>priložnost za obisk glamurozne<br class="sp"> zabave pod nebom.</strong>
							</p>
							<p>
								ČESTITAMO! Skupaj gremo do vrha! 
							</p>
							<p>
								Vabimo te v <strong>Nebo Show Restaurant.</strong><br>
								Na glamurozni zabavi nad mestom se<br class="sp">
								dobimo <strong>28. 3. 2023.</strong>
							</p>
							<p>
								<strong>Potrdi udeležbo</strong> in označi, ali potrebuješ<br>
								tudi <strong>prevoz.</strong> S prijavo ne odlašaj, saj je<br class="sp">
								število mest na dogodku <strong>zelo omejeno.</strong>
							</p>
						</div>
						<div class="action-wrapper">
							<div class="action-group">
								<button type="button" class="btn primary" @click="{ comming = true; }" :class="{ selected: comming }">Pridem</button>
								<button type="button" class="btn primary" @click="{ comming = false; confirmed = 2}">Ne Pridem</button>
							</div>
							<div class="action-group animate-fadeIn" v-if="comming === true">
								<button type="button" class="btn primary" @click="needRide = true" :class="{ selected: needRide }">POTREBUJEM <br class="sp"> PREVOZ</button>
								<button type="button" class="btn primary" @click="needRide = false; confirmed = 1; confirm()">Imam svoj <br class="sp"> prevoz</button>
							</div>
							<div class="action-group animate-fadeIn" v-if="needRide">
								<select class="form-control form-select" name="prevoz" id="prevoz" @change="setStation($event)">
									<option v-for="(prevoz, index) in prevozi" :value=prevoz.value>{{ prevoz.text }}</option>
								</select>
								<span class="drop-arrow"><img src="../assets/images/down.png"></span>
							</div>						
						</div>
					</div>	
				</div>
				<div class="entry-station-result animate-fadeIn" v-else-if="currentQuestionIndex >= questions.length && confirmed > 0">
					<div class="content">
						<Logo/>
						<div class="description">
							<div v-if="confirmed == 1" v-html="stations[0].correct"></div>
							<div v-else v-html="stations[0].incorrect"></div>
						</div>
						<div class="action-wrapper">
							<a href="https://nas-partner.si/" class="btn primary">nazaj na portal</a>
						</div>
					</div>				
				</div>
			</div>
		</div>
		<div class="quiz-completed animate-fadeIn" v-else-if="available === true">
			<Logo/>			
			<div class="hero-banner text-center">
				<img src="../assets/images/sky_logo.svg" alt="Quiz">
			</div>
			<div class="description text-center">
				<p>
					Glamurozno. Znova ti je uspelo dokazati,<br class="sp">
					da <strong>tvoje znanje o glamurozni znamki<br class="sp">
					zares blesti.</strong>
				</p>
				<p>
					Žal so <strong>prosta mesta na GLAM<br class="sp">
					dogodku že zapolnjena,</strong> še vedno pa<br class="sp">
					tvoj ostaja <strong>unikaten žig.</strong> <br class="pc">V primeru, da se<br class="sp">
					kakšno izmed zapolnjenih mest <strong>sprosti,</strong> te<br class="sp">
					lahko o tem <strong>nemudoma obvestimo.</strong>
				</p>
				<p>
					V kolikor te udeležba na dogodku zanima,<br class="sp">
					klikni na spodnji gumb in se vpiši na seznam<br class="sp"> 
					<strong>VIP rezerv.</strong>
				</p>				
			</div>
			<div class="action-wrapper">
				<button class="btn primary" @click="completed">ZANIMAM SE ZA OBISK<br>GLAM DOGODKA</button>
			</div>
			<div class="description text-center">
				<p>
					V vsakem primeru pa zate pripravljamo še<br class="sp">
					veliko glamuroznih dogodkov.
				</p>
				<p>
					Lepo se imej<br>
					tvoj Glamour
				</p>
			</div>				
		</div>
	</div>
</template>

<script>
import Logo from './Logo.vue'
import Vue from 'vue';
import axios from 'axios';

export default {
	name: 'Quiz',
	components: {
		Logo
	},
	data() {
		return {
			isStarted: false,
			isEntryStation: 0,
			selectedStation: '',
			available: null,
			confirmed: '',
			questions: [],
			stations: [],
			alphabets: [],
			currentQuestionResult: {},
			chosenAnswers: [],
			chosenStation: [],
			chosenAnswerExactness: 0,
			currentQuestionIndex: 0,
			currentStateSelectionResult: '',
			comming: false,
			needRide: false,
			customErrMsg: '',
			error: false,
			prevoz: '',
			prevozi: [
				{ value: '', text: 'IZBERI VSTOPNO POSTAJO', disabled: true },
				{ value: 'NOVA GORICA', text: 'NOVA GORICA' },
				{ value: 'AJDOVŠČINA', text: 'AJDOVŠČINA' },
				{ value: 'POSTOJNA', text: 'POSTOJNA' },
				{ value: 'BREŽICE', text: 'BREŽICE' },
				{ value: 'KRŠKO', text: 'KRŠKO' },
				{ value: 'NOVO MESTO', text: 'NOVO MESTO' },
				{ value: 'TREBNJE', text: 'TREBNJE' },
				{ value: 'MURSKA SOBOTA', text: 'MURSKA SOBOTA' },
				{ value: 'MARIBOR', text: 'MARIBOR' },
				{ value: 'CELJE', text: 'CELJE' },
				{ value: 'JESENICE', text: 'JESENICE' },
				{ value: 'RADOVLJICA', text: 'RADOVLJICA' },
				{ value: 'KRANJ', text: 'KRANJ' },
			],
		};
	},
	mounted() {
		this.init();

		// check event availability
		axios.get('/api/glamour/event-get-availability')
		.then((res) => {
			if (res.data.available == '1') this.available = true
			else this.available = false
		})
		.catch((error) => {
		  this.available = false;
		  console.log(error);
        });
	},
	methods: {
		init() {
			window.Vue = Vue;
			
			this.questions = [
				{
					id: 1,
					description: 'Glamour št. <span>1</span> ostaja zahvaljujoč<br class="sp"> svojemu raznolikemu asortimanu.<br> Najbolj priljubljena izbira<br class="sp"> polnoletnih kadilk so cigarete<br class="sp"> Glamour <br class="pc"> Amber.',
					question: 'Glamour št. <span>1</span> ostaja zahvaljujoč<br class="sp"> svojemu raznolikemu asortimanu.<br> Najbolj priljubljena izbira<br class="sp"> polnoletnih kadilk so cigarete<br> Glamour Amber.',
					correct_answer: 'Drži',
					answers: [
						'Drži', 'Ne Drži'
					],
					result: {
						correct: {
							title: 'Glamurozna izbira.',
							description: 'Drži, najbolj priljubljena izbira polnoletnih<br class="sp"> kadilk so cigarete Glamour Amber.',
						},
						incorrect: {
							title: 'Ups, napačen odgovor.',
							description: 'Cigarete Glamour Amber so namreč<br class="sp"> najbolj priljubljena izbira polnoletnih kadilk.',
						}
					},
					product_info: {
						image_path: 'amber.png',
						name: 'Amber',
						tar_amount: 1,
						price: "4.40"
					}
				},
				{
					id: 2,
					description: 'Glamour na vrhu ostaja, ker ponuja<br class="sp"> sijajno kvaliteto in dostopno ceno.<br> Škatlico Glamour cigaret lahko<br class="sp"> polnoletne kadilke dobijo za 4.50 €.',
					question: '',
					correct_answer: 'Ne Drži',
					answers: [
						'Drži', 'Ne Drži'
					],
					result: {
						correct: {
							title: 'Odlično znanje.',
							description: 'Škatlico Glamour cigaret lahko namreč<br class="sp"> polnoletne kadilke dobijo po dostopni<br class="sp"> ceni 4.40 €.'
						},
						incorrect: {
							title: 'Žal tokrat trditev<br> ne drži.',
							description: 'Škatlico Glamour cigaret lahko namreč<br class="sp"> polnoletne kadilke dobijo po dostopni ceni<br class="sp"> 4.40 €.'
						}
					}
				},
				{
					id: 3,
					description: 'Glamour znamka je znana po tem,<br class="sp"> da lahko zadovolji želje vsake<br> polnoletne kadilke. Ženski kadilki, ki<br class="sp"> želi najbolj močan okus kajenja, zato ponudiš cigarete Glamour Lilac.',
					question: '',
					correct_answer: 'Drži',
					answers: [
						'Drži', 'Ne Drži'
					],
					result: {
						correct: {
							title: 'Tvoje znanje je sijajno.',
							description: 'Drži, za kadilke, ki želijo najmočnejši okus<br class="sp"> kajenja, je najboljša izbira<br class="sp"> Glamour Lilac (5 mg).'
						},
						incorrect: {
							title: 'Ups, zastavljena trditev<br> je tokrat držala.',
							description: 'Za kadilke, ki želijo najmočnejši okus<br class="sp"> kajenja, je najboljša izbira<br class="sp"> Glamour Lilac (5 mg).'
						}
					},
					product_info: {
						image_path: 'lilac.png',
						name: 'Lilac',
						tar_amount: 5,
						price: "4.40"
					}
				}
			];
			
			this.stations = [
				{
					'correct': '<p>Sijajno! Komaj čakamo, da se skupaj s tabo podružimo na GLAM zabavi nad Ljubljano. Več informacij o samem dogodku boš kmalu prejel/-a na svoj elektronski naslov.</p> <p>Se vidimo, <br>tvoj Glamour</p>',
					'incorrect': '<p>Nič hudega, se bomo pa srečali na enem izmed prihajajočih dogodkov.</p> <p>Še naprej ti želimo sijajen dan,<br> tvoj Glamour</p>',
				}
			];

			for (let i = 0; i < 26; i++) {
				this.alphabets.push(String.fromCharCode(i + 97).toUpperCase());				
			}
		},
		startQuiz() {
	      	this.isStarted = true;
			this.scrollToTop();
		},
		selectAnswer(index) {      	
			let checkState = (this.questions[this.currentQuestionIndex].correct_answer == this.questions[this.currentQuestionIndex].answers[index]);
			
			let str = (checkState) ? 'correct' : 'incorrect';
			this.currentQuestionResult = this.questions[this.currentQuestionIndex].result[str];
			window.Vue.set(this.chosenAnswers, this.currentQuestionIndex, checkState);

			this.scrollToTop();
    	},
		selectStation(index) {
			console.log(index)
			if (index == 3 && this.isEntryStation != 1) {
				this.isEntryStation = 0;
				this.currentStateSelectionResult = '';
			} else {
				let str = (index == 1) ? 'correct' : 'incorrect';

				if (index != 3) {
					this.currentStateSelectionResult = this.stations[0][str];
				}

				this.isEntryStation = index;
			}

			this.scrollToTop();
		},
		nextQuestion() {			
			this.currentQuestionResult = {};
			this.currentQuestionIndex++;

			this.scrollToTop();
		},
		setStation(e) {
			this.selectedStation == e.target.value;
			if (e.target.value != '') {				
				this.confirmed = 1;
				this.confirm();
			}			

			this.scrollToTop();
		},
		scrollToTop() {
			console.log('redghhost');
			window.scrollTo({ top: 0 });
		},
		confirm() {
			let pickupStation = '...'
			
			if (!this.comming) {
				return
			} else if (this.needRide && this.prevoz != '') pickupStation = this.prevoz
			else pickupStation = 'lasten prevoz'

			let t = !this.needRide ? 'own' : 'dill';

			axios.post('/api/glamour/event-signup', {
				transport: t,
				prevoz: pickupStation
			}).then((res) => {
				console.log(res?.data?.msg);
			})
			.catch((err) => {
				let errMsg = err.response?.data?.msg
				if (errMsg == 'already_participated' || errMsg == 'no_more_places') {
					this.customErrMsg = err.response.data.customErrMsg
				} else {
					this.error = true
				}
			});
		},
		completed() {
			axios.post('/api/glamour/event-signup', {
				transport: 'Koper',				
			}).then((res) => {
				console.log(res?.data?.msg);
			})
			.catch((err) => {
				let errMsg = err.response?.data?.msg
				if (errMsg == 'already_participated' || errMsg == 'no_more_places') {
					this.customErrMsg = err.response.data.customErrMsg
				} else {	
					this.error = true
				}
			});
		}
	}	
}
</script>
