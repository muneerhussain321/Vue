<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart()"> Add to Cart</button>
    <div class="top-row">
      <div class="top part" :class="{'sale-border':selectedRobort.head.onSale}">
        <div class="robot-name">
           {{ selectedRobort.head.title }}
           <span v-if="selectedRobort.head.onSale" class="sale">OnSale!</span>
        </div>
        <img :src="selectedRobort.head.src" title="head"/>
        <button @click="selectpreviuosHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobort.leftArm.src" title="left arm"/>
        <button @click="selectpreviuosLeftArm()" class="prev-selector">&#9650;</button>
        <button @click="selectNextLeftArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobort.torso.src" title="left arm"/>
        <button @click="selectpreviuostorsos()" class="prev-selector">&#9668;</button>
        <button @click=" selectNexttorsos()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobort.right.src" title="left arm"/>
        <button @click="selectPreviousRighttArm()"  class="prev-selector">&#9650;</button>
        <button @click="selectNextRightArm()"  class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobort.bases.src" title="left arm"/>
        <button @click="selectPreviousBases()" class="prev-selector">&#9668;</button>
        <button @click="selectNextBases()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <table>
        <thead>
          <th>Robort</th>
          <th class="cost">Cost</th>
        </thead>
        <tbody>
          <tr v-for="(robot,index) in cart" :key="index">
            <td>{{ robot.head.title }}</td>
            <td class="cost">{{ robot.cost }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

</template>
<script>
import avaliableParts from '../data/parts';

function getNextValidIndex(index, length) {
  const increamentedIndex = index + 1;
  return increamentedIndex > length - 1 ? 0 : increamentedIndex;
}
function getPreviousValidIndex(index, length) {
  const deprecatedIndex = index - 1;
  return deprecatedIndex < 0 ? length - 1 : deprecatedIndex;
}

export default {
  name: 'RobortBuilder',
  data() {
    return {
      cart: [],
      avaliableParts,
      selectNextHeadIndex: 0,
      selectNextLeftArmIndex: 0,
      selectNextRightArmIndex: 0,

      selectNextTorsosIndex: 0,
      selectNextBasesIndex: 0,
    };
  },
  computed: {
    headBorderStyle() {
      return { border: this.selectedRobort.head.onSale ? '3px solid red' : '3px solid #aaa' };
    },
    selectedRobort() {
      return {
        head: avaliableParts.heads[this.selectNextHeadIndex],
        leftArm: avaliableParts.arms[this.selectNextLeftArmIndex],
        right: avaliableParts.arms[this.selectNextRightArmIndex],
        torso: avaliableParts.torsos[this.selectNextTorsosIndex],
        bases: avaliableParts.bases[this.selectNextBasesIndex],
      };
    },
  },
  methods: {
    addToCart() {
      const Robort = this.selectedRobort;
      const cost = Robort.head.cost + Robort.leftArm.cost
      + Robort.torso.cost + Robort.right.cost + Robort.bases.cost;
      this.cart.push({ ...Robort, cost });
    },
    selectNextHead() {
      // this.selectNextHeadIndex += 1;
      // console.log(this.selectNextHeadIndex);
      this.selectNextHeadIndex
      = getNextValidIndex(this.selectNextHeadIndex, avaliableParts.heads.length);
      console.log(this.selectNextHeadIndex);
    },
    selectpreviuosHead() {
      this.selectNextHeadIndex
      = getPreviousValidIndex(this.selectNextHeadIndex, avaliableParts.heads.length);
      console.log(this.selectNextHeadIndex);
    },
    selectNextLeftArm() {
      // this.selectNextHeadIndex += 1;
      // console.log(this.selectNextHeadIndex);
      this.selectNextLeftArmIndex
      = getNextValidIndex(this.selectNextLeftArmIndex, avaliableParts.arms.length);
      // console.log(this.selectNextHeadIndex);
    },
    selectpreviuosLeftArm() {
      this.selectNextLeftArmIndex
      = getPreviousValidIndex(this.selectNextLeftArmIndex, avaliableParts.arms.length);
      // console.log(this.selectNextHeadIndex);
    },

    selectNextRightArm() {
      // this.selectNextHeadIndex += 1;
      // console.log(this.selectNextHeadIndex);
      this.selectNextRightArmIndex
      = getNextValidIndex(this.selectNextRightArmIndex, avaliableParts.arms.length);
      // console.log(this.selectNextHeadIndex);
    },
    selectPreviousRighttArm() {
      this.selectNextRightArmIndex
      = getPreviousValidIndex(this.selectNextRightArmIndex, avaliableParts.arms.length);
    },
    selectNexttorsos() {
      this.selectNextTorsosIndex
      = getNextValidIndex(this.selectNextTorsosIndex, avaliableParts.torsos.length);
    },
    selectpreviuostorsos() {
      this.selectNextTorsosIndex
      = getPreviousValidIndex(this.selectNextTorsosIndex, avaliableParts.torsos.length);
    },

    // selectNextBasesIndex
    selectNextBases() {
      this.selectNextBasesIndex
      = getNextValidIndex(this.selectNextBasesIndex, avaliableParts.bases.length);
    },
    selectPreviousBases() {
      this.selectNextBasesIndex
      = getPreviousValidIndex(this.selectNextBasesIndex, avaliableParts.bases.length);
    },
  },
};
</script>
<style scoped>
.part {
  position: relative;
  width:165px;
  height:165px;
  border: 3px solid #aaa;
}
.part img {
  width:165px;
}
.top-row {
  display:flex;
  justify-content: space-around;
}
.middle-row {
  display:flex;
  justify-content: center;
}
.bottom-row {
  display:flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector, .center .next-selector {
  opacity:0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}
.robot-name{
  position:absolute;
  top:-25px;
  text-align: center;
  width: 100%
}
.sale{
  color: red;
}
.content{
  position: relative;
}
.add-to-cart{
  position:absolute;
  right: 30px;
  widows: 220;
  padding: 3px;
  font-size: 16px;
}
td,th{
text-align: left;
padding: 5px;
padding-right: 5px;
}
.cost{
  text-align: right;
}
.sale-border{
  border: 3px solid red;
}
</style>
