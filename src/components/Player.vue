<template>
  <div></div>
</template>
<script lang="ts">
import { Component, Prop } from 'vue-property-decorator'

import * as THREE from 'three'

import BaseMesh from './BaseMesh.vue'
import { GLTFLoader } from '../utils/GLTFLoader.js';
import * as SkeletonUtils from '../utils/SkeletonUtils.js';

@Component({
  name: 'Ball',
  components: {}
})
export default class Player extends BaseMesh {
  @Prop() protected teamColor!: string


  protected material!: THREE.MeshPhongMaterial

  // computed
  get radius() {
    return this.frustumSize / 50
  }

  mounted() {
    //this.initialize()
    this.loadRobot()
  }

  loadRobot() {
    const loader = new GLTFLoader();
    const scene = this.scene;

    loader.load( '../assets/robot/RobotExpressive.glb', function ( robot:any ) {

      robot.scene.traverse( function ( object:any ) {

        if ( object.isMesh ) object.castShadow = true;

      } );
      const model = SkeletonUtils.clone( robot.scene );
      model.scale.set(.2,.2,.2);
      model.position.set( 0, 0, 0 );
      model.name = 'robot';
      model.index = 1;
      scene.add( model );
      // const mixer = new THREE.AnimationMixer( model );
      // const actions:object = {};
      // for ( let i = 0; i < robot.animations.length; i++ ) {

			// 		const clip:object = robot.animations[ i ];  // 获取其中一个动画
			// 		const action:object = mixer.clipAction( clip );  // 返回动画操作对象
			// 		actions[ clip.name ] = action;  // 关联动画名称和动作

			// 		if ( emotes.indexOf( clip.name ) >= 0 || states.indexOf( clip.name ) >= 4 ) {

			// 			action.clampWhenFinished = true;  	//暂停在最后一帧播放的状态
			// 			action.loop = THREE.LoopOnce;  		//不循环播放

			// 		}

			// 	}
    } );
  }

  generateGeometry(): THREE.CylinderGeometry {
    const radiusTop = this.radius // ui: radiusTop
    const radiusBottom = this.radius // ui: radiusBottom
    const height = this.radius // ui: height
    const radialSegments = 12 // ui: radialSegments
    return new THREE.CylinderGeometry(radiusTop, radiusBottom, height, radialSegments)
  }

  generateMaterial(): THREE.MeshPhongMaterial {
    return new THREE.MeshPhongMaterial({ color: this.teamColor })
  }

  configMesh() {
    this.mesh.castShadow = true
    this.mesh.position.set(20, this.radius, 0)
  }
}
</script>
<style scoped lang="scss"></style>
