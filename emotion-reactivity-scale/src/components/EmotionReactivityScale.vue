<script setup>
import { ref, computed } from "vue";

const patientName = ref("");
const dateSubmitted = ref("");

const scaleLabels = [
	{ value: 0, label: "Jamais" },
	{ value: 1, label: "Quelques fois par an" },
	{ value: 2, label: "Environ 1 fois par mois" },
	{ value: 3, label: "2–3 fois par mois" },
	{ value: 4, label: "Environ 1 fois par semaine" },
	{ value: 5, label: "2–3 fois par semaine" },
	{ value: 6, label: "Tous les jours" },
];

const questions = [
	"When something happens that upsets me, it's all I can think about for a long time.",
	"My feelings get hurt easily.",
	"When I experience emotions, I feel them very strongly/intensely.",
	"When I'm emotionally upset, my whole body gets physically upset as well.",
	"I tend to get very emotional very easily.",
	"I experience emotions very strongly.",
	"I often feel extremely anxious.",
	"When I feel emotional, it's hard for me to imagine feeling any other way.",
	"Even the littlest things make me emotional.",
	"If I have a disagreement with someone, it takes a long time for me to get over it.",
	"When I am angry/upset, it takes me much longer than most people to calm down.",
	"I get angry at people very easily.",
	"I am often bothered by things that other people don't react to.",
	"I am easily agitated.",
	"My emotions go from neutral to extreme in an instant.",
	"When something bad happens, my mood changes very quickly. People tell me I have a very short fuse.",
	"People tell me that my emotions are often too intense for the situation.",
	"I am a very sensitive person.",
	"My moods are very strong and powerful.",
	"I often get so upset it's hard for me to think straight.",
	"Other people tell me I'm overreacting.",
];

const answers = ref(Array(questions.length).fill(null));

const answeredCount = computed(() => answers.value.filter((a) => a !== null).length);

const allAnswered = computed(() => answeredCount.value === questions.length);
</script>

<template>
	<div class="min-h-screen bg-white p-4 sm:p-6 lg:p-8">
		<div class="max-w-3xl mx-auto">
			<!-- Header -->
			<h1 class="text-xl sm:text-2xl font-bold text-center text-gray-900 mb-6">Emotion Reactivity Scale</h1>

			<!-- Instructions -->
			<div class="border border-gray-300 rounded-lg p-4 mb-6 bg-gray-50">
				<p class="text-sm text-gray-700"><span class="font-semibold">Instructions:</span> Veuillez vous auto-évaluer sur les affirmations suivantes, en utilisant une échelle de 0 à 6.</p>
			</div>

			<!-- Scale Legend -->
			<div class="mb-4 p-3 bg-gray-100 rounded-lg">
				<div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-7 gap-2 text-xs text-gray-600">
					<div v-for="item in scaleLabels" :key="item.value" class="flex items-center gap-1">
						<span class="font-semibold">{{ item.value }}</span> = {{ item.label }}
					</div>
				</div>
			</div>

			<!-- Questions Table -->
			<div class="border border-gray-300 rounded-lg overflow-hidden">
				<!-- Desktop Header -->
				<div class="hidden lg:grid lg:grid-cols-[1fr_repeat(7,44px)] bg-gray-100 border-b border-gray-300">
					<div class="p-3"></div>
					<div v-for="item in scaleLabels" :key="item.value" class="p-2 text-center font-semibold text-gray-700 text-sm">
						{{ item.value }}
					</div>
				</div>

				<!-- Questions -->
				<div v-for="(question, index) in questions" :key="index" class="border-b border-gray-200 last:border-b-0">
					<!-- Desktop Layout -->
					<div class="hidden lg:grid lg:grid-cols-[1fr_repeat(7,44px)] items-center">
						<div class="p-3 text-sm text-gray-700">{{ index + 1 }}. {{ question }}</div>
						<div v-for="item in scaleLabels" :key="item.value" class="p-2 flex justify-center">
							<label class="cursor-pointer">
								<input type="radio" :name="`question-${index}`" :value="item.value" v-model="answers[index]" class="w-5 h-5 text-blue-600 border-gray-300 focus:ring-blue-500 cursor-pointer" />
								<span class="sr-only">{{ item.label }}</span>
							</label>
						</div>
					</div>

					<!-- Mobile/Tablet Layout -->
					<div class="lg:hidden p-4">
						<p class="text-sm text-gray-700 mb-3">{{ index + 1 }}. {{ question }}</p>
						<div class="flex justify-center gap-2 sm:gap-3">
							<label v-for="item in scaleLabels" :key="item.value" class="cursor-pointer flex flex-col items-center">
								<input type="radio" :name="`question-mobile-${index}`" :value="item.value" v-model="answers[index]" class="w-5 h-5 sm:w-6 sm:h-6 text-blue-600 border-gray-300 focus:ring-blue-500 cursor-pointer" />
								<span class="text-xs text-gray-500 mt-1">{{ item.value }}</span>
							</label>
						</div>
					</div>
				</div>
			</div>

			<!-- Progress Display -->
			<div class="mt-6 p-4 bg-gray-50 border border-gray-200 rounded-lg">
				<p class="text-center text-gray-700">{{ answeredCount }} / {{ questions.length }} questions répondues</p>
			</div>

			<!-- Submit Button -->
			<div class="mt-6 text-center">
				<button
					:disabled="!allAnswered"
					class="px-6 py-3 bg-blue-600 text-white font-medium rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 disabled:bg-gray-400 disabled:cursor-not-allowed transition-colors">
					Soumettre
				</button>
			</div>
		</div>
	</div>
</template>
