<template>
  <div class="tools">
    <div class="tools-title">
      叉车等效均布荷载计算
      <el-popover placement="bottom" title="图例" width="600" trigger="hover">
        <div slot="reference" class="shadow" style="margin-left: 10px; width: 20px; height: 20px; line-height: 20px; font-size: 12px; border-radius: 50%; background-color: white">
          ?
        </div>
        <el-row :gutter="10">
          <el-col :span="12"
            ><div>叉车运行方向与板跨平行</div>
            <el-image :src="require('@/assets/img/shuipin.jpg')"></el-image
          ></el-col>
          <el-col :span="12"
            ><div>叉车运行方向与板跨垂直</div>
            <el-image :src="require('@/assets/img/chuizhi.jpg')"></el-image
          ></el-col>
        </el-row>
      </el-popover>
    </div>

    <div class="tools-form">
      <el-row :gutter="10">
        <el-col :span="12">
          <el-card class="box-card" shadow="hover">
            <div slot="header" class="tools-form-title">
              <span>叉车资料</span>
              <el-button style="float: right; padding: 3px 0" type="text" @click="clear1">清零</el-button>
            </div>
            <el-form>
              <el-row :gutter="10">
                <el-col :span="12">
                  <el-form-item prop="Pf" label-width="80px" label="前轴载重">
                    <el-input size="mini" v-model="shuju.Pf">
                      <span slot="suffix">kN</span>
                      <span slot="prefix" style="font-size: 16px">P<span style="font-size: 10px">前</span></span></el-input
                    >
                  </el-form-item></el-col
                >
                <el-col :span="12">
                  <el-form-item prop="Pb" label-width="80px" label="后轴载重"
                    ><el-input size="mini" v-model="shuju.Pb">
                      <span slot="suffix">kN</span>
                      <span slot="prefix" style="font-size: 16px">P<span style="font-size: 10px">后</span></span>
                    </el-input>
                  </el-form-item></el-col
                >
              </el-row>
              <el-row :gutter="10">
                <el-col :span="12">
                  <el-form-item prop="e" label-width="80px" label="前轴轮距">
                    <el-input size="mini" v-model="shuju.e"> <span slot="suffix">m</span><span slot="prefix" style="font-size: 16px">e</span></el-input>
                  </el-form-item></el-col
                >
                <el-col :span="12">
                  <el-form-item prop="c" label-width="80px" label="轴距">
                    <el-input size="mini" v-model="shuju.c"><span slot="suffix">m</span><span slot="prefix" style="font-size: 16px">c</span></el-input>
                  </el-form-item></el-col
                >
              </el-row>
            </el-form>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="box-card" shadow="hover">
            <div slot="header" class="tools-form-title">
              <span>假设叉车轮胎接地尺寸</span>
              <el-button style="float: right; padding: 3px 0" type="text" @click="clear2">清零</el-button>
            </div>
            <el-form>
              <el-row :gutter="10">
                <el-col :span="12">
                  <el-form-item prop="length" label-width="80px" label="着地长">
                    <el-input size="mini" v-model="shuju.length"> <span slot="suffix">m</span><span slot="prefix" style="font-size: 12px">大值</span></el-input>
                  </el-form-item></el-col
                >
                <el-col :span="12">
                  <el-form-item prop="width" label-width="80px" label="着地宽"
                    ><el-input size="mini" v-model="shuju.width"><span slot="suffix">m</span><span slot="prefix" style="font-size: 12px">小值</span></el-input>
                  </el-form-item></el-col
                >
              </el-row>
            </el-form>
          </el-card>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-card class="box-card" shadow="hover">
            <div slot="header" class="tools-form-title">
              <span>结构资料</span>
              <el-button style="float: right; padding: 3px 0" type="text" @click="clear3">清零</el-button>
            </div>
            <el-form>
              <el-row :gutter="10">
                <el-col :span="12">
                  <el-form-item prop="h" label-width="80px" label="楼板厚度">
                    <el-input size="mini" v-model="shuju.h">
                      <span slot="suffix">m</span>
                      <span slot="prefix" style="font-size: 16px">h</span>
                    </el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item prop="L" label-width="80px" label="次梁间距">
                    <el-input size="mini" v-model="shuju.L">
                      <span slot="suffix">m</span>
                      <span slot="prefix" style="font-size: 16px">L</span>
                    </el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row :gutter="10">
                <el-col :span="12">
                  <el-form-item prop="s" label-width="80px" label="面层厚度">
                    <el-input size="mini" v-model="shuju.s">
                      <span slot="suffix">m</span>
                      <span slot="prefix" style="font-size: 16px">s</span>
                    </el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item prop="xs" label-width="80px" label="动力系数">
                    <el-input-number size="mini" style="width: 100%" v-model="shuju.xs" :min="1.1" :max="1.3" :precision="2" :step="0.01" label="描述文字"></el-input-number>
                  </el-form-item>
                </el-col>
              </el-row>
            </el-form>
          </el-card>
        </el-col>
      </el-row>
      <el-row style="height: calc(100% - 338px)">
        <el-col :span="24" style="height: 100%">
          <el-card class="box-card scroll-box" shadow="hover" style="height: 100%">
            <div slot="header" class="tools-form-title">
              <span>工况计算</span>
              <div style="float: right">
                <el-button type="text" @click="clearAll">全部清零</el-button>
              </div>
            </div>
            <div class="result">
              <div style="font-weight: 600">一、叉车运行方向与楼板跨度方向平行时</div>
              <div><span>合力中心距前轴距离</span> a {{ result.a ? '=' + result.a + 'm' : '' }}</div>

              <el-row :gutter="10">
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 1、跨中最大弯矩 M<span style="font-size: 10px">max</span> </template>
                    <el-descriptions-item>
                      <template slot="label"> 跨中最大弯矩 M<span style="font-size: 10px">max</span> </template>
                      {{ result.Mmax ? result.Mmax + 'kNm' : '' }}
                    </el-descriptions-item>
                  </el-descriptions>
                </el-col>
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 2、有效分布宽度 b </template>
                    <el-descriptions-item>
                      <template slot="label"> 平行于板跨计算宽度 b<span style="font-size: 10px">tx</span> </template>
                      {{ result.btx ? result.btx + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item>
                      <template slot="label"> 垂直于板跨计算宽度 b<span style="font-size: 10px">ty</span> </template>{{ result.bty ? result.bty + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="平行板跨计算宽度">
                      <template slot="label"> 平行板跨计算宽度 b<span style="font-size: 10px">cx</span> </template>{{ result.bcx ? result.bcx + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="垂直板跨计算宽度">
                      <template slot="label"> 垂直板跨计算宽度 b<span style="font-size: 10px">cy</span> </template>{{ result.bcy ? result.bcy + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="简支板上有效分布荷载 b">{{ result.b ? result.b + 'm' : '' }} </el-descriptions-item>
                    <el-descriptions-item label="荷载相邻折减分布宽度 b'">{{ result.b2 ? result.b2 + 'm' : '' }} </el-descriptions-item>
                  </el-descriptions>
                </el-col>
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 3、等效均布荷载q<span style="font-size: 10px">e</span></template>
                    <el-descriptions-item>
                      <template slot="label">动力系数前 q<span style="font-size: 10px">e</span></template>
                      {{ result.qe ? result.qe + 'kPa' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item>
                      <template slot="label">动力系数后 q<span style="font-size: 10px">e</span>'</template>
                      {{ result.qe2 ? result.qe2 + 'kPa' : '' }}
                    </el-descriptions-item>
                  </el-descriptions>
                </el-col>
              </el-row>
            </div>
            <div class="result" style="margin-top: 10px">
              <div style="font-weight: 600">一、叉车运行方向与楼板跨度方向平行时</div>
              <div><span>合力中心距前轴距离</span> a {{ result2.a ? '=' + result2.a + 'm' : '' }}</div>

              <el-row :gutter="10">
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 1、跨中最大弯矩 M<span style="font-size: 10px">max</span> </template>
                    <el-descriptions-item>
                      <template slot="label"> 跨中最大弯矩 M<span style="font-size: 10px">max</span> </template>
                      {{ result2.Mmax ? result2.Mmax + 'kNm' : '' }}
                    </el-descriptions-item>
                  </el-descriptions>
                </el-col>
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 2、有效分布宽度 b </template>
                    <el-descriptions-item>
                      <template slot="label"> 平行于板跨计算宽度 b<span style="font-size: 10px">tx</span> </template>
                      {{ result2.btx ? result2.btx + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item>
                      <template slot="label"> 垂直于板跨计算宽度 b<span style="font-size: 10px">ty</span> </template>{{ result2.bty ? result2.bty + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="平行板跨计算宽度">
                      <template slot="label"> 平行板跨计算宽度 b<span style="font-size: 10px">cx</span> </template>{{ result2.bcx ? result2.bcx + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="垂直板跨计算宽度">
                      <template slot="label"> 垂直板跨计算宽度 b<span style="font-size: 10px">cy</span> </template>{{ result2.bcy ? result2.bcy + 'm' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item label="简支板上有效分布荷载 b">{{ result2.b ? result2.b + 'm' : '' }} </el-descriptions-item>
                    <el-descriptions-item label="荷载相邻折减分布宽度 b'">{{ result2.b2 ? result2.b2 + 'm' : '' }} </el-descriptions-item>
                  </el-descriptions>
                </el-col>
                <el-col :span="8">
                  <el-descriptions class="margin-top" :column="1" size="mini" border>
                    <template slot="title"> 3、等效均布荷载q<span style="font-size: 10px">e</span></template>
                    <el-descriptions-item>
                      <template slot="label">动力系数前 q<span style="font-size: 10px">e</span></template>
                      {{ result2.qe ? result2.qe + 'kPa' : '' }}
                    </el-descriptions-item>
                    <el-descriptions-item>
                      <template slot="label">动力系数后 q<span style="font-size: 10px">e</span>'</template>
                      {{ result2.qe2 ? result2.qe2 + 'kPa' : '' }}
                    </el-descriptions-item>
                  </el-descriptions>
                </el-col>
              </el-row>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
export default {
  name: 'tools',
  data() {
    return {
      shuju: {
        Pf: null, //B9
        Pb: null, //B10
        e: null, //B11
        c: null, //B12
        length: null, //B14
        width: null, //B15
        h: null, //B17
        L: null, //B18
        s: null, //B19
        xs: 1.1
      },
      // result: {
      //   a: null, //C22
      //   Mmax: null,
      //   btx: null,
      //   bty: null,
      //   bcx: null,
      //   bcy: null,
      //   b: null,
      //   b2: null,
      //   qe: null,
      //   qe2: null
      // },
      radio: '1'
    }
  },
  computed: {
    result() {
      return this.getResult('1')
    },
    result2() {
      return this.getResult('2')
    }
  },
  props: [],
  methods: {
    clear1() {
      this.shuju.Pf = null
      this.shuju.Pb = null
      this.shuju.e = null
      this.shuju.c = null
    },
    clear2() {
      this.shuju.length = null
      this.shuju.width = null
    },
    clear3() {
      this.shuju.h = null
      this.shuju.L = null
      this.shuju.s = null
    },
    clearAll() {
      this.shuju = {
        Pf: null, //B9
        Pb: null, //B10
        e: null, //B11
        c: null, //B12
        length: null, //B14
        width: null, //B15
        h: null, //B17
        L: null, //B18
        s: null, //B19
        xs: 1.1
      }
    },
    getResult(radio) {
      let shuju = {}
      for (const key in this.shuju) {
        if (Object.hasOwnProperty.call(this.shuju, key)) {
          const element = this.shuju[key]
          this.$set(shuju, key, element != null ? Number(element) : null)
        }
      }
      let a = shuju.Pb && shuju.c && shuju.Pf ? (shuju.Pb * shuju.c) / (shuju.Pf + shuju.Pb) : null
      let Mmax = null
      if (shuju.Pf && shuju.Pb && shuju.L && a && radio == '1') {
        Mmax = ((shuju.Pf + shuju.Pb) * Math.pow(shuju.L - a, 2)) / (8 * shuju.L)
      } else if (shuju.Pf && shuju.Pb && shuju.L && a && radio == '2') {
        Mmax = (shuju.Pf * Math.pow(shuju.L - shuju.e / 2, 2)) / (4 * shuju.L)
      } else {
        Mmax = null
      }
      let btx
      if (radio == '1') {
        btx = shuju.width
      } else if (radio == '2') {
        btx = shuju.length
      } else {
        btx = null
      }
      let bty
      if (radio == '1') {
        bty = shuju.length
      } else if (radio == '2') {
        bty = shuju.width
      } else {
        bty = null
      }
      let bcx
      if (btx && shuju.s && shuju.h) {
        bcx = btx + 2 * shuju.s + shuju.h
      } else {
        bcx = null
      }
      let bcy
      if (bty && shuju.s && shuju.h) {
        bcy = bty + 2 * shuju.s + shuju.h
      } else {
        bcy = null
      }
      let b
      if (radio == '1' && shuju.L && bcy) {
        if (bcy <= 2.2 * shuju.L) {
          b = (bcy * 2) / 3 + 0.73 * shuju.L
        } else {
          b = bcy
        }
      } else if (radio == '2' && shuju.L && bcy) {
        b = bcy + 0.7 * shuju.L
      } else {
        b = null
      }
      let b2
      if (radio == '1' && shuju.e && b) {
        if (shuju.e < b) {
          b2 = (b + shuju.e) / 2
        } else {
          b2 = b
        }
      } else if (radio == '2') {
        if (shuju.c < b) {
          b2 = (shuju.c + b) / 2
        } else {
          b2 = b
        }
      } else {
        b2 = null
      }
      let qe
      if (Mmax && b2 && shuju.L) {
        qe = (8 * Mmax) / (b2 * Math.pow(shuju.L, 2))
      } else {
        qe = null
      }
      let qe2
      if (shuju.xs && qe) {
        qe2 = shuju.xs * qe
      } else {
        qe2 = null
      }
      let param = {
        a: a,
        Mmax: Mmax,
        btx: btx,
        bty: bty,
        bcx: bcx,
        bcy: bcy,
        b: b,
        b2: b2,
        qe: qe,
        qe2: qe2
      }
      for (const key in param) {
        if (Object.hasOwnProperty.call(param, key)) {
          const element = param[key]
          param[key] = element ? Math.round(element * 10000).toFixed(4) / 10000 : null
        }
      }
      return param
    }
  },
  mounted() {}
}
</script>
<style lang="scss" scoped>
.tools {
  height: 100%;
  width: 100%;
  // overflow-y: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  &-title {
    text-align: center;
    display: flex;
    align-items: center;
    // margin-top: 20px;
    font-size: 30px;
    line-height: 45px;
    color: #663300;
    font-family: SiYuanM;
    font-weight: 600;
    text-shadow: 0px 0px 1px #333;
  }
  &-form {
    width: 100%;
    height: calc(100% - 45px);
    padding: 10px;
    overflow-y: auto;
    &-title {
      font-weight: 600;
      padding-left: 10px;
    }
    &-btn {
      margin-top: 20px;
      text-align: center;
    }

    .result {
      font-size: 14px;
    }
  }
}
.box-card {
  height: 159px;
  /deep/.el-card__body,
  /deep/.el-card__header {
    padding: 10px;
  }
}
.el-row {
  margin-bottom: 10px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-icon-question {
  color: teal;
  cursor: pointer;
}
.el-button {
  padding: 0;
}
/deep/.el-icon-plus:before {
  content: '+';
  font-size: 21px;
  line-height: 22px;
}
/deep/.el-icon-minus:before {
  content: '-';
  font-size: 21px;
  line-height: 22px;
}
/deep/.el-descriptions__title {
  font-size: 14px;
}
/deep/.el-descriptions-item__label {
  width: 50%;
}
/deep/.el-descriptions__header {
  margin-bottom: 10px;
}
.scroll-box {
  /deep/.el-card__body {
    overflow-y: auto;
    height: calc(100% - 44.2px);
  }
}
</style>