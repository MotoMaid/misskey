<!--
SPDX-FileCopyrightText: syuilo and other misskey contributors
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
	<div>
		<div>
			<span style="font-size: 1.6em;"></span>
			<b>{{ title }}</b>
		</div>
		<div>
			<div :class="$style.model">	
				<model-viewer 
					ref="modelViewerRef"	
					:src="modelUrl"
					poster="/client-assets/view-3dmodel.png"
					camera-controls
					loading="lazy"
					reveal="manual"
					autoplay
					@click="loadModel"
					@contextmenu.stop
					@load="handleModelLoad"
					>
				</model-viewer>
			</div>
		</div>
		<!-- TODO: 最大化するボタンの設置 -->
		<!-- <button 
			:class="$style.menu" 
			class="_button" 
			@click.stop="OpenInNewWindow">
			<i class="ti ti-dots" style="vertical-align: middle;">
			</i>
		</button> -->
	</div>
</template>

<script lang="ts" setup>
import * as misskey from 'misskey-js';
import '@google/model-viewer';
import { ref, computed } from 'vue';
import bytes from '@/filters/bytes.js';

// import ModelView from '@/components/MkMediaModelView.vue';

const title = computed(() => `${props.model.name}  [${bytes(props.model.size)}]`);

// モデルの読み込み状態を保持するref
const modelLoaded = ref(false);

const isModelLoaded = computed(() => modelLoaded.value);

function handleModelLoad() {
  modelLoaded.value = true;
}

const props = defineProps<{
	model: misskey.entities.DriveFile;
}>();

const modelUrl = ref(props.model.url);

function loadModel(event) {
  const modelViewer = event.currentTarget;
  modelViewer.dismissPoster();
}

// TODO: 外部ウィンドウ開くか内部ウィンドウで開く処理
function OpenInNewWindow(ev: MouseEvent) {
}



</script>

<style lang="scss" module>
.model {
	display: flex;
	justify-content: center;
	align-items: center;
	font-size: 3.5em;
	overflow: hidden;
	background-position: center;
	background-size: cover;
	width: 100%;
	height: 100%;
}

// TODO: 複数のメディアがアップロードされても適切なサイズになるようにする
model-viewer {
	width: 100%;
	height: 340px;
}	

.menu {
	display: block;
	position: absolute;
	border-radius: 999px;
	background-color: rgba(0, 0, 0, 0.3);
	-webkit-backdrop-filter: var(--blur, blur(15px));
	backdrop-filter: var(--blur, blur(15px));
	color: #fff;
	font-size: 0.8em;
	width: 28px;
	height: 28px;
	text-align: center;
	bottom: 10px;
	right: 10px;
}

</style>
