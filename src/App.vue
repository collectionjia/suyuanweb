<template>
  <div id="app">
    <div class="container">
      <!-- 系统头部 -->
      <header class="system-header">
        <h1 class="system-title">中药材生产全程可追溯管理系统</h1>
      </header>
      
      <!-- 主要内容 -->
      <main class="system-content">
        <!-- 药材基本信息卡片 -->
        <div class="info-card">
          <h2 class="card-title">药材基本信息</h2>
          <div class="info-grid">
            <div class="info-item">
              <label>药材名称：</label>
              <span class="info-value">{{ basicInfo["药材名称"] }}</span>
            </div>
            <div class="info-item">
              <label>经营主体：</label>
              <span class="info-value">{{ basicInfo["经营主体"] }}</span>
            </div>
            <div class="info-item">
              <label>地块总数量：</label>
              <span class="info-value">{{ basicInfo["地块总数量（个）"] }} 个</span>
            </div>
            <div class="info-item">
              <label>地块总面积：</label>
              <span class="info-value">{{ basicInfo["地块总面积（亩）"] }} 亩</span>
            </div>
          </div>
        </div>
        
        <!-- 生产环节导航 -->
        <div class="process-nav">
          <h2 class="card-title">生产环节信息</h2>
          <div class="process-container">
            <!-- 选地信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(0)">
                <span class="process-name">选地信息</span>
                <span class="expand-icon">{{ expandedProcesses[0] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[0]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['选地信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="key === '地块实景图' && value && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <img v-for="(img, index) in value" :key="index" :src="img" :alt="`地块实景图 ${index + 1}`" class="field-image">
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 整地信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(1)">
                <span class="process-name">整地信息</span>
                <span class="expand-icon">{{ expandedProcesses[1] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[1]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['整地信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="(key === '整地图片' || key === '底肥包装袋图片') && value && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <img v-for="(img, index) in value" :key="index" :src="img" :alt="`${key} ${index + 1}`" class="field-image">
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 种植信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(2)">
                <span class="process-name">种植信息</span>
                <span class="expand-icon">{{ expandedProcesses[2] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[2]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['种植信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="Array.isArray(value) && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <template v-if="key === '视频上传'">
                          <video v-for="(video, index) in value" :key="index" :src="video" controls class="field-video"></video>
                        </template>
                        <template v-else>
                          <img v-for="(img, index) in value" :key="index" :src="img" :alt="`${key} ${index + 1}`" class="field-image">
                        </template>
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 田管信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(3)">
                <span class="process-name">田管信息</span>
                <span class="expand-icon">{{ expandedProcesses[3] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[3]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['田管信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="Array.isArray(value) && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <template v-if="key.includes('视频')">
                          <video v-for="(video, index) in value" :key="index" :src="video" controls class="field-video"></video>
                        </template>
                        <template v-else>
                          <img v-for="(img, index) in value" :key="index" :src="img" :alt="`${key} ${index + 1}`" class="field-image">
                        </template>
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 采收信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(4)">
                <span class="process-name">采收信息</span>
                <span class="expand-icon">{{ expandedProcesses[4] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[4]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['采收信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="Array.isArray(value) && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <template v-if="key.includes('视频')">
                          <video v-for="(video, index) in value" :key="index" :src="video" controls class="field-video"></video>
                        </template>
                        <template v-else>
                          <img v-for="(img, index) in value" :key="index" :src="img" :alt="`${key} ${index + 1}`" class="field-image">
                        </template>
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 加工信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(5)">
                <span class="process-name">加工信息</span>
                <span class="expand-icon">{{ expandedProcesses[5] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[5]" class="process-details">
                <div class="info-grid">
                  <div class="info-item" v-for="(value, key) in processDetails['加工信息']" :key="key">
                    <label>{{ key }}：</label>
                    <span v-if="Array.isArray(value) && value.length > 0" class="info-value">
                      <div class="image-gallery">
                        <img v-for="(img, index) in value" :key="index" :src="img" :alt="`${key} ${index + 1}`" class="field-image">
                      </div>
                    </span>
                    <span v-else class="info-value">{{ value }}</span>
                  </div>
                </div>
                
                <!-- 加工流程数据 -->
                <div class="process-flow">
                  <h3 style="font-size: 18px; font-weight: bold; color: #28a745; margin-top: 20px; margin-bottom: 15px;">加工流程</h3>
                  <div v-if="isLoadingFlowData" style="text-align: center; padding: 20px; color: #6c757d;">加载中...</div>
                  <div v-else-if="processingFlowData && processingFlowData.list && processingFlowData.list.length > 0">
                    <table style="width: 100%; border-collapse: collapse; margin-top: 10px;">
                      <thead>
                        <tr style="background-color: #f8f9fa;">
                          <th style="padding: 10px; text-align: left; border-bottom: 2px solid #dee2e6;">步骤序号</th>
                          <th style="padding: 10px; text-align: left; border-bottom: 2px solid #dee2e6;">步骤名称</th>
                          <th style="padding: 10px; text-align: left; border-bottom: 2px solid #dee2e6;">操作</th>
               
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(item, index) in processingFlowData.list" :key="item.fid" style="border-bottom: 1px solid #dee2e6;">
                          <td style="padding: 10px;">{{ index +1}}</td>
                          <td style="padding: 10px;">{{ item.input_odax }}</td>
                          <td style="padding: 10px;">{{ item.input_k34r }}</td> 
                        </tr>
                      </tbody>
                    </table>
                  </div>
                  <div v-else style="text-align: center; padding: 20px; color: #6c757d;">暂无加工流程数据</div>
                </div>
              </div>
            </div>
            
            <!-- 贮藏信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(6)">
                <span class="process-name">贮藏信息</span>
                <span class="expand-icon">{{ expandedProcesses[6] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[6]" class="sub-processes">
                 
              </div>
            </div>
            
            <!-- 运输信息 -->
            <div class="process-item">
              <div class="process-header" v-on:click="toggleProcess(7)">
                <span class="process-name">运输信息</span>
                <span class="expand-icon">{{ expandedProcesses[7] ? '▼' : '▶' }}</span>
              </div>
              <div v-show="expandedProcesses[7]" class="sub-processes">
               
              </div>
            </div>
          </div>
        </div>
      </main>
      
      <!-- 页脚 -->
      <footer class="system-footer">
        <p>&copy; 中药材生产全程可追溯管理系统. 保留所有权利.</p>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function() {
    return {
      // 控制每个生产环节的展开状态
      expandedProcesses: [false, false, false, false, false, false, false, false],
      // 接口返回的数据
      apiData: null,
      // 加工流程数据
      processingFlowData: null,
      // 加工流程数据加载状态
      isLoadingFlowData: false,
      // 字段映射关系
      fieldMapping: {
        "input_a7ej1": "经营主体",
        "input_a1tq1": "地块总数量（个）",
        "input_uaev1": "地块总面积（亩）",
        "input_o54g": "地块编号",
        "input_ofwb": "药材名称",
        "input_wwr5": "地块承包者",
        "input_qdtm": "地块经营者",
        "input_yfp8": "详细地址",
        "textarea_p1hy": "选址标准",
        "input_0yxj": "选地面积（亩）",
        "input_9b47": "海拔（m）",
        "input_1r52": "经度",
        "input_1rpr": "纬度",
        "input_j3de": "土壤pH值",
        "input_89jo": "前茬作物",
        "input_pgxd": "坡度等级",
        "input_yxwa": "土壤地质",
        "ImageUpload_v0nu": "地块实景图",
        "input_f10c": "地块编号",
        "input_8rvu": "负责人姓名",
        "input_fac2": "负责人联系电话",
        "input_ccp7": "操作者姓名",
        "date_3x9p": "整地开始与结束时间",
        "input_x0xu": "整地标准",
        "input_2geg": "整地工具及型号",
        "input_b9ch": "耕地深度（cm）",
        "input_om1v": "耕耙次数（次）",
        "input_5ccj": "厢宽（cm）",
        "input_h8xz": "厢沟宽（cm）",
        "input_8uh0": "单体面积（亩）",
        "ImageUpload_6fgd": "整地图片",
        "date_umy9": "施用开始与结束日期",
        "input_0rq8": "施肥量（kg/亩）",
        "input_57gk": "肥料品名",
        "input_tjr0": "标称有效成分及含量",
        "input_n5ck": "生产厂家",
        "input_5j93": "厂家联系方式",
        "ImageUpload_pe6h": "底肥包装袋图片",
        "input_e879": "地块编号",
        "date_x9g8": "播种开始与结束日期",
        "input_8um3": "播种负责人",
        "input_7k6x": "播种用种量(kg/亩)",
        "input_tjrg": "播放方式",
        "input_ralk": "播种面积",
        "ImageUpload_1l1h": "播后覆前图片",
        "FileUpload_x8uf": "视频上传",
        "date_4uky": "育苗开始与结束日期",
        "input_r29q": "育苗负责人",
        "input_zgbj": "育苗数量(亩)",
        "input_bg63": "育苗面积(亩)",
        "input_30cg": "预计移栽面积(亩)",
        "ImageUpload_haj1": "移栽实景图",
        "input_cez2": "设施(灌溉/遮阴/棚架)",
        "input_7kpx": "搭建信息",
        "ImageUpload_wfp2": "设备及工具图片",
        "ImageUpload_7kna": "设施图片",
        "input_1zmk": "学名",
        "input_u0dz": "分类地位(科、属)",
        "input_8zqr": "药用部位",
        "ImageUpload_8yuv": "基原鉴定证明性文件扫描件",
        "ImageUpload_b3tt": "药材鲜品、初加工品及饮片特征图片",
        "ImageUpload_299x": "药材不同的生长阶段形态图",
        "input_mqul": "种子生产者单位",
        "input_97ux": "种苗生产者单位",
        "input_14ll": "种苗生产者姓名",
        "input_y544": "种苗生产者电话",
        "input_mp14": "种苗总量(株)",
        "input_p40p": "种苗规格",
        "date_sky9": "种苗起挖开始与结束时间",
        "input_r3rs": "贮存环境",
        "input_yxef": "可种面积(亩)",
        "ImageUpload_ykmz": "种源实物图",
        "input_dcyo": "种源规格",
        "ImageUpload_j5sx": "种源证明材料扫描件",
        "input_aryl": "地块编号",
        "input_m4lm": "排灌/遮阴/搭棚设施建造及维护操作标准",
        "input_w8ly": "排灌设施使用时间及维护内容",
        "input_qdsp": "遮阴设施使用时间及维护内容",
        "input_6jnf": "棚架设施使用时间及维护内容",
        "input_baa7": "异常操作信息",
        "ImageUpload_6lr8": "设施建造场景图片",
        "ImageUpload_e6ej": "设施使用维护场景图片",
        "ImageUpload_pt6m": "出苗/移栽实际操作场景图片",
        "ImageUpload_qnql": "田管操作场景图(含针对异常情况的农事操作)",
        "input_09w1": "农事名称",
        "input_o4z1": "农事负责人",
        "date_4qwb": "农事开始与结束时间",
        "textarea_qrzx": "农事内容",
        "textarea_0eug": "农事标准",
        "ImageUpload_cnbp": "现场操作图片",
        "date_582t": "投入品开始与结束时间",
        "ImageUpload_97as": "投入品操作图片",
        "FileUpload_l1xp": "投入品视频上传",
        "input_d88x": "采收批次",
        "date_vvft": "采收开始与结束时间",
        "input_m3sf": "采收部位",
        "input_qeaa": "采收年限",
        "input_kna2": "采收负责人",
        "input_r9hu": "采收面积(亩)",
        "input_vshn": "采收数量(kg)",
        "input_ye7u": "采收方式",
        "input_axza": "采收工具及设备",
        "input_ryxs": "临时保存方法与时间",
        "input_ab1j": "采收标准",
        "ImageUpload_2xqf": "临时保存现场图片",
        "ImageUpload_n4bx": "采收相关设备图片",
        "ImageUpload_ugy7": "采收现场图片",
        "FileUpload_6u9p": "采收视频上传",
        "input_shkg": "加工批号",
        "date_fxu0": "加工开始与结束时间",
        "input_cuh7": "加工类型",
        "input_gy9l": "加工负责人",
        "input_zr2c": "采收编号",
        "input_8ffa": "鲜品数量(kg)",
        "input_1hts": "成品数量(kg)",
        "input_8j42": "加工工具及设备",
        "input_ajuk": "加工工艺及要求",
        "input_oh0z": "加工流程图",
        "ImageUpload_vr26": "加工现场图片",
        "ImageUpload_h6a2": "加工设备图片",
        "ImageUpload_ay2k": "药检报告图",
        "input_yp0t": "包装标准",
        "input_d9sn": "包装材质规格",
        "input_x381": "包装设备",
        "input_l3hd": "包装负责人",
        "input_2z96": "包装件数(件)",
        "input_6uv7": "单件质量(kg)",
        "ImageUpload_fqzp": "包装后成品图",
        "ImageUpload_8z81": "包装合格证明",
        "ImageUpload_n5lc": "包装设备的实图",
        "fcreationdate": "创建时间",
        "fcreatebyname": "创建人",
        "flastupdatedate": "修改时间",
        "flastupdatebyname": "修改人"
      },
      // 基本信息数据
      basicInfo: {
        "经营主体": "",
        "地块总数量（个）": "",
        "地块总面积（亩）": ""
      },
      // 各环节详细数据
      processDetails: {
        "选地信息": {},
        "整地信息": {},
        "种植信息": {},
        "田管信息": {},
        "采收信息": {},
        "加工信息": {},
        "贮藏信息": {},
        "运输信息": {}
      }
    }
  },
  mounted: function() {
    this.fetchData();
  },
  methods: {
    // 拼接媒体资源地址（接口返回相对路径）
    mediaUrl: function(path) {
      if (!path) return '';
      const p = String(path).trim();
      if (/^https?:\/\//i.test(p)) return p;
      return 'http://39.100.16.228:8081' + (p.startsWith('/') ? p : '/' + p);
    },
    mediaList: function(value, sep) {
      if (!value) return [];
      const parts = String(value).split(sep || ',').map(function(s) { return s.trim(); }).filter(Boolean);
      const self = this;
      return parts.map(function(p) { return self.mediaUrl(p); });
    },
    // 从接口获取数据（新后台公开溯源接口）
    fetchData: function() {
      const urlParams = new URLSearchParams(window.location.search);
      const fid = urlParams.get('fid') || "2077671651931066368";
      const url = 'http://39.100.16.228:8081/api/public/trace/' + encodeURIComponent(fid);

      this.isLoadingFlowData = true;
      fetch(url)
        .then(response => response.json())
        .then(data => {
          if (data.code !== 0) {
            console.error('Error fetching data:', data.message || data.msg);
            return;
          }
          this.apiData = data;
          this.mapData();
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        })
        .finally(() => {
          this.isLoadingFlowData = false;
        });
    },
    // 映射数据
    mapData: function() {
      if (!this.apiData || !this.apiData.data) return;

      // 新接口字段在 data.detail 中
      const data = this.apiData.data.detail || this.apiData.data;

      // 映射基本信息（兼容新旧字段名）
      this.basicInfo["经营主体"] = data["input_a7ej"] || data["input_a7ej1"] || this.apiData.data["经营主体"] || "";
      this.basicInfo["地块总数量（个）"] = data["input_a1tq"] || data["input_a1tq1"] || "";
      this.basicInfo["地块总面积（亩）"] = data["input_uaev"] || data["input_uaev1"] || "";
      this.basicInfo["药材名称"] = data["input_ofwb"] || this.apiData.data["药品名称"] || "";

      // 加工流程嵌在 listview_qyfo
      let flow = data["listview_qyfo"];
      if (typeof flow === 'string') {
        try { flow = JSON.parse(flow); } catch (e) { flow = []; }
      }
      this.processingFlowData = { list: Array.isArray(flow) ? flow : [] };
      
      // 映射选地信息
      this.processDetails["选地信息"] = {
        "地块编号": data["input_o54g"] || "",
        "药材名称": data["input_ofwb"] || "",
        "地块承包者": data["input_wwr5"] || "",
        "地块经营者": data["input_qdtm"] || "",
        "详细地址": data["input_yfp8"] || "",
        "选址标准": data["textarea_p1hy"] || "",
        "选地面积（亩）": data["input_0yxj"] || "",
        "海拔（m）": data["input_9b47"] || "",
        "经度": data["input_1r52"] || "",
        "纬度": data["input_1rpr"] || "",
        "土壤pH值": data["input_j3de"] || "",
        "前茬作物": data["input_89jo"] || "",
        "坡度等级": data["input_pgxd"] || "",
        "土壤地质": data["input_yxwa"] || "",
        "地块实景图": data["ImageUpload_v0nu"] ? data["ImageUpload_v0nu"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : []
      };
      
      // 映射整地信息
      this.processDetails["整地信息"] = {
        "地块编号": data["input_f10c"] || "",
        "负责人姓名": data["input_8rvu"] || "",
        "负责人联系电话": data["input_fac2"] || "",
        "操作者姓名": data["input_ccp7"] || "",
        "整地开始与结束时间": data["date_3x9p"] || "",
        "整地标准": data["input_x0xu"] || "",
        "整地工具及型号": data["input_2geg"] || "",
        "耕地深度（cm）": data["input_b9ch"] || "",
        "耕耙次数（次）": data["input_om1v"] || "",
        "厢宽（cm）": data["input_5ccj"] || "",
        "厢沟宽（cm）": data["input_h8xz"] || "",
        "单体面积（亩）": data["input_8uh0"] || "",
        "整地图片": data["ImageUpload_6fgd"] ? data["ImageUpload_6fgd"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "施用开始与结束日期": data["date_umy9"] || "",
        "施肥量（kg/亩）": data["input_0rq8"] || "",
        "肥料品名": data["input_57gk"] || "",
        "标称有效成分及含量": data["input_tjr0"] || "",
        "生产厂家": data["input_n5ck"] || "",
        "厂家联系方式": data["input_5j93"] || "",
        "底肥包装袋图片": data["ImageUpload_pe6h"] ? data["ImageUpload_pe6h"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : []
      };
      
      // 映射种植信息
      this.processDetails["种植信息"] = {
        "地块编号": data["input_e879"] || "",
        "播种开始与结束日期": data["date_x9g8"] || "",
        "播种负责人": data["input_8um3"] || "",
        "播种用种量(kg/亩)": data["input_7k6x"] || "",
        "播放方式": data["input_tjrg"] || "",
        "播种面积": data["input_ralk"] || "",
        "播后覆前图片": data["ImageUpload_1l1h"] ? data["ImageUpload_1l1h"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "视频上传": data["FileUpload_x8uf"] ? data["FileUpload_x8uf"].split(';').filter(url => url.trim()).map(url => `http://39.100.16.228:8081/dev-api/${url.replace(/^\/dev-api\//, '')}`) : [],
        "育苗开始与结束日期": data["date_4uky"] || "",
        "育苗负责人": data["input_r29q"] || "",
        "育苗数量(亩)": data["input_zgbj"] || "",
        "育苗面积(亩)": data["input_bg63"] || "",
        "预计移栽面积(亩)": data["input_30cg"] || "",
        "移栽实景图": data["ImageUpload_haj1"] ? data["ImageUpload_haj1"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "设施(灌溉/遮阴/棚架)": data["input_cez2"] || "",
        "搭建信息": data["input_7kpx"] || "",
        "设备及工具图片": data["ImageUpload_wfp2"] ? data["ImageUpload_wfp2"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "设施图片": data["ImageUpload_7kna"] ? data["ImageUpload_7kna"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "学名": data["input_1zmk"] || "",
        "分类地位(科、属)": data["input_u0dz"] || "",
        "药用部位": data["input_8zqr"] || "",
        "基原鉴定证明性文件扫描件": data["ImageUpload_8yuv"] ? data["ImageUpload_8yuv"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "药材鲜品、初加工品及饮片特征图片": data["ImageUpload_b3tt"] ? data["ImageUpload_b3tt"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "药材不同的生长阶段形态图": data["ImageUpload_299x"] ? data["ImageUpload_299x"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "种子生产者单位": data["input_mqul"] || "",
        "种苗生产者单位": data["input_97ux"] || "",
        "种苗生产者姓名": data["input_14ll"] || "",
        "种苗生产者电话": data["input_y544"] || "",
        "种苗总量(株)": data["input_mp14"] || "",
        "种苗规格": data["input_p40p"] || "",
        "种苗起挖开始与结束时间": data["date_sky9"] || "",
        "贮存环境": data["input_r3rs"] || "",
        "可种面积(亩)": data["input_yxef"] || "",
        "种源实物图": data["ImageUpload_ykmz"] ? data["ImageUpload_ykmz"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "种源规格": data["input_dcyo"] || "",
        "种源证明材料扫描件": data["ImageUpload_j5sx"] ? data["ImageUpload_j5sx"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : []
      };
      
      // 映射田管信息
      this.processDetails["田管信息"] = {
        "地块编号": data["input_aryl"] || "",
        "排灌/遮阴/搭棚设施建造及维护操作标准": data["input_m4lm"] || "",
        "排灌设施使用时间及维护内容": data["input_w8ly"] || "",
        "遮阴设施使用时间及维护内容": data["input_qdsp"] || "",
        "棚架设施使用时间及维护内容": data["input_6jnf"] || "",
        "异常操作信息": data["input_baa7"] || "",
        "设施建造场景图片": data["ImageUpload_6lr8"] ? data["ImageUpload_6lr8"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "设施使用维护场景图片": data["ImageUpload_e6ej"] ? data["ImageUpload_e6ej"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "出苗/移栽实际操作场景图片": data["ImageUpload_pt6m"] ? data["ImageUpload_pt6m"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "田管操作场景图(含针对异常情况的农事操作)": data["ImageUpload_qnql"] ? data["ImageUpload_qnql"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "农事名称": data["input_09w1"] || "",
        "农事负责人": data["input_o4z1"] || "",
        "农事开始与结束时间": data["date_4qwb"] || "",
        "农事内容": data["textarea_qrzx"] || "",
        "农事标准": data["textarea_0eug"] || "",
        "现场操作图片": data["ImageUpload_cnbp"] ? data["ImageUpload_cnbp"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "投入品开始与结束日期": data["date_582t"] || "",
        "投入品操作图片": data["ImageUpload_97as"] ? data["ImageUpload_97as"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "投入品视频上传": data["FileUpload_l1xp"] ? data["FileUpload_l1xp"].split(';').filter(url => url.trim()).map(url => `http://39.100.16.228:8081/dev-api/${url.replace(/^\/dev-api\//, '')}`) : [],
        // 新增字段
        "农事名称2": data["input_phv4"] || "",
        "农事负责人2": data["input_eyrh"] || "",
        "农事开始与结束时间2": data["date_p4n2"] || "",
        "农事内容2": data["textarea_ch5r"] || "",
        "农事标准2": data["textarea_a5ns"] || "",
        "现场操作图片2": data["ImageUpload_r5ga"] ? data["ImageUpload_r5ga"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "投入品视频上传2": data["FileUpload_90jw"] ? data["FileUpload_90jw"].split(';').filter(url => url.trim()).map(url => `http://39.100.16.228:8081/dev-api/${url.replace(/^\/dev-api\//, '')}`) : []
      };
      
      // 映射采收信息
      this.processDetails["采收信息"] = {
        "采收批次": data["input_d88x"] || "",
        "采收开始与结束时间": data["date_vvft"] || "",
        "采收部位": data["input_m3sf"] || "",
        "采收年限": data["input_qeaa"] || "",
        "采收负责人": data["input_kna2"] || "",
        "采收面积(亩)": data["input_r9hu"] || "",
        "采收数量(kg)": data["input_vshn"] || "",
        "采收方式": data["input_ye7u"] || "",
        "采收工具及设备": data["input_axza"] || "",
        "临时保存方法与时间": data["input_ryxs"] || "",
        "采收标准": data["input_ab1j"] || "",
        "临时保存现场图片": data["ImageUpload_2xqf"] ? data["ImageUpload_2xqf"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "采收相关设备图片": data["ImageUpload_n4bx"] ? data["ImageUpload_n4bx"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "采收现场图片": data["ImageUpload_ugy7"] ? data["ImageUpload_ugy7"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "采收视频上传": data["FileUpload_6u9p"] ? data["FileUpload_6u9p"].split(';').filter(url => url.trim()).map(url => `http://39.100.16.228:8081/dev-api/${url.replace(/^\/dev-api\//, '')}`) : []
      };
      
      // 映射加工信息
      this.processDetails["加工信息"] = {
        "加工批号": data["input_shkg"] || "",
        "加工开始与结束时间": data["date_fxu0"] || "",
        "加工类型": data["input_cuh7"] || "",
        "加工负责人": data["input_gy9l"] || "",
        "采收编号": data["input_zr2c"] || "",
        "鲜品数量(kg)": data["input_8ffa"] || "",
        "成品数量(kg)": data["input_1hts"] || "",
        "加工工具及设备": data["input_8j42"] || "",
        "加工工艺及要求": data["input_ajuk"] || "",
        "加工流程图": data["input_oh0z"] || "",
        "加工现场图片": data["ImageUpload_vr26"] ? data["ImageUpload_vr26"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "加工设备图片": data["ImageUpload_h6a2"] ? data["ImageUpload_h6a2"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "药检报告图": data["ImageUpload_ay2k"] ? data["ImageUpload_ay2k"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "包装标准": data["input_yp0t"] || "",
        "包装材质规格": data["input_d9sn"] || "",
        "包装设备": data["input_x381"] || "",
        "包装负责人": data["input_l3hd"] || "",
        "包装件数(件)": data["input_2z96"] || "",
        "单件质量(kg)": data["input_6uv7"] || "",
        "包装后成品图": data["ImageUpload_fqzp"] ? data["ImageUpload_fqzp"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "包装合格证明": data["ImageUpload_8z81"] ? data["ImageUpload_8z81"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : [],
        "包装设备的实图": data["ImageUpload_n5lc"] ? data["ImageUpload_n5lc"].split(',').map(img => `http://39.100.16.228:8081/${img}`) : []
      };
    },
    // 加载并播放视频

    // 切换生产环节的展开/折叠状态
    toggleProcess: function(index) {
      this.$set(this.expandedProcesses, index, !this.expandedProcesses[index]);
    }
  }
}
</script>

<style scoped>
/* 页面容器样式 */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
  line-height: 1.6;
}

/* 系统标题样式 */
.system-title {
  font-size: 28px;
  font-weight: bold;
  color: #28a745;
  text-align: center;
  margin-bottom: 30px;
  padding-bottom: 15px;
  border-bottom: 2px solid #e3f2fd;
}

/* 药材基本信息卡片样式 */
.info-card {
  background-color: white;
  border-radius: 8px;
  padding: 25px;
  margin-bottom: 30px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

/* 卡片标题 */
.card-title {
  font-size: 22px;
  font-weight: bold;
  color: #28a745;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 2px solid #e3f2fd;
}

/* 信息网格布局 */
.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

/* 信息项 */
.info-item {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.info-item label {
  font-size: 14px;
  color: #6c757d;
  font-weight: 500;
}

.info-value {
  font-size: 16px;
  color: #343a40;
  font-weight: bold;
}

/* 地块实景图样式 */
.field-image {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  border: 1px solid #e9ecef;
  margin-top: 5px;
}

/* 图片画廊样式，支持多张图片分割显示 */
.image-gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 5px;
}

/* 生产环节导航卡片样式 */
.process-nav {
  background-color: white;
  border-radius: 8px;
  padding: 25px;
  margin-bottom: 30px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

/* 生产环节项目样式 */
.process-item {
  margin-bottom: 10px;
  border-radius: 6px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.process-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  background: linear-gradient(135deg, #28a745, #20c997);
  color: white;
  font-weight: 600;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.process-header:hover {
  background: linear-gradient(135deg, #218838, #1e7e34);
  box-shadow: 0 4px 15px rgba(40, 167, 69, 0.3);
}

.process-name {
  flex: 1;
}

.expand-icon {
  font-size: 14px;
  transition: transform 0.3s ease;
}

/* 展开详情样式 */
.process-details {
  background-color: #f8f9fa;
  padding: 20px;
  border-left: 3px solid #28a745;
  transition: all 0.3s ease;
}

/* 小类样式 */
.sub-processes {
  background-color: #f8f9fa;
  padding: 10px 0;
  border-left: 3px solid #28a745;
  transition: all 0.3s ease;
}

.sub-process-item {
  padding: 8px 0;
}

.sub-process-link {
  display: block;
  padding: 10px 40px;
  color: #495057;
  text-decoration: none;
  font-size: 15px;
  transition: all 0.3s ease;
  border-bottom: 1px solid #e9ecef;
}

.sub-process-link:hover {
  background-color: rgba(40, 167, 69, 0.1);
  color: #28a745;
  padding-left: 45px;
}

.sub-process-item:last-child .sub-process-link {
  border-bottom: none;
}

/* 页脚 */
.system-footer {
  text-align: center;
  margin-top: 40px;
  padding: 20px;
  color: #6c757d;
  font-size: 14px;
  border-top: 1px solid #e9ecef;
}

.field-video {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  margin: 5px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .container {
    padding: 10px;
  }
  
  .system-title {
    font-size: 24px;
  }
  
  .info-grid {
    grid-template-columns: 1fr;
  }
  
  .info-card,
  .process-nav {
    padding: 20px;
  }
  
  .card-title {
    font-size: 20px;
  }
  
  .process-header {
    padding: 14px 16px;
    font-size: 15px;
  }
  
  .sub-process-link {
    padding: 10px 30px;
    font-size: 14px;
  }
  
  .sub-process-link:hover {
    padding-left: 35px;
  }
  
  /* 视频样式 */
  .field-video {
    flex: 1 1 300px;
    max-width: 100%;
    min-width: 200px;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin: 5px;
  }
}
</style>
