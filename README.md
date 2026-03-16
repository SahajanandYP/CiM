<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BEV-LIO Project Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --navy:   #09111F;
  --navy2:  #0E1A2E;
  --navy3:  #142238;
  --navy4:  #1A2D47;
  --glass:  rgba(255,255,255,0.04);
  --border: rgba(255,255,255,0.08);
  --border2:rgba(255,255,255,0.14);
  --white:  #FFFFFF;
  --dim:    rgba(255,255,255,0.45);
  --dimmer: rgba(255,255,255,0.25);
  --purple: #7B5CF0; --purple-l:#EDE8F9; --purple-d:#3A2A80;
  --teal:   #10B981; --teal-l:  #D0F2E8; --teal-d:  #065842;
  --blue:   #3B82F6; --blue-l:  #DBEAFE; --blue-d:  #1E3A8A;
  --gold:   #F59E0B; --gold-l:  #FEF3C7; --gold-d:  #78350F;
  --coral:  #EF4444; --coral-l: #FEE2E2; --coral-d: #7F1D1D;
  --green:  #22C55E; --green-l: #DCFCE7; --green-d: #14532D;
  --pink:   #EC4899; --pink-l:  #FCE7F3; --pink-d:  #831843;
  --done:   #22C55E;
  --inprog: #F59E0B;
  --todo:   rgba(255,255,255,0.2);
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{background:var(--navy);font-family:'DM Sans',sans-serif;color:var(--white);min-height:100vh;overflow-x:hidden}

/* ── TOPBAR ── */
.topbar{
  position:sticky;top:0;z-index:200;
  background:rgba(9,17,31,0.92);
  backdrop-filter:blur(16px);
  border-bottom:1px solid var(--border);
  padding:14px 32px;
  display:flex;align-items:center;justify-content:space-between;
  gap:16px;
}
.topbar-left{display:flex;flex-direction:column;gap:2px}
.topbar-title{
  font-family:'Space Mono',monospace;
  font-size:16px;font-weight:700;letter-spacing:-0.3px;
}
.topbar-title span{color:var(--gold)}
.topbar-sub{font-size:11px;color:var(--dimmer);letter-spacing:0.5px}
.topbar-right{display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.pill-btn{
  display:inline-flex;align-items:center;gap:6px;
  font-family:'Space Mono',monospace;font-size:10px;font-weight:700;
  letter-spacing:0.5px;padding:7px 14px;border-radius:30px;
  cursor:pointer;border:1px solid var(--border2);
  background:var(--glass);color:var(--white);
  transition:all .15s;white-space:nowrap;
}
.pill-btn:hover{background:var(--navy4);border-color:rgba(255,255,255,0.3)}
.pill-btn.accent{background:var(--gold);color:var(--navy);border-color:var(--gold)}
.pill-btn.accent:hover{opacity:.9}
.pill-btn.danger{background:var(--coral);color:#fff;border-color:var(--coral)}

/* ── PROGRESS OVERVIEW ── */
.overview{
  padding:24px 32px 0;
  display:flex;gap:12px;flex-wrap:wrap;
}
.ov-card{
  background:var(--navy3);border:1px solid var(--border);
  border-radius:12px;padding:14px 20px;
  display:flex;flex-direction:column;gap:4px;
  min-width:120px;
}
.ov-num{font-family:'Space Mono',monospace;font-size:26px;font-weight:700;line-height:1}
.ov-lbl{font-size:11px;color:var(--dim);letter-spacing:0.3px}
.progress-bar-wrap{flex:1;min-width:220px;background:var(--navy3);border:1px solid var(--border);border-radius:12px;padding:14px 20px;display:flex;flex-direction:column;gap:8px}
.progress-bar-track{height:8px;background:rgba(255,255,255,0.08);border-radius:30px;overflow:hidden}
.progress-bar-fill{height:100%;border-radius:30px;background:linear-gradient(90deg,var(--purple),var(--blue),var(--green));transition:width .5s ease}
.progress-bar-lbl{display:flex;justify-content:space-between;font-size:11px;color:var(--dim)}

/* ── TIMELINE CONTAINER ── */
.timeline-wrap{padding:32px 32px 60px;display:flex;flex-direction:column;gap:0}

/* ── PHASE ROW ── */
.phase-row{display:flex;gap:0;position:relative;padding-bottom:0}

/* spine */
.spine{
  display:flex;flex-direction:column;align-items:center;
  width:64px;flex-shrink:0;
}
.node-circle{
  width:44px;height:44px;border-radius:50%;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  font-family:'Space Mono',monospace;font-size:9px;font-weight:700;
  flex-shrink:0;cursor:pointer;position:relative;z-index:2;
  border:2px solid transparent;transition:transform .2s,box-shadow .2s;
  letter-spacing:0.3px;
}
.node-circle:hover{transform:scale(1.12)}
.node-ico{font-size:16px;line-height:1}
.node-num{font-size:8px;margin-top:1px;opacity:0.8}
.spine-line{width:2px;flex:1;min-height:24px;opacity:0.25;transition:opacity .3s}

/* content area */
.phase-content{
  flex:1;margin-left:16px;
  padding-bottom:32px;
}

/* ── PHASE CARD ── */
.p-card{
  background:var(--navy2);
  border:1px solid var(--border);
  border-radius:16px;overflow:hidden;
  transition:border-color .2s;
}
.p-card:hover{border-color:var(--border2)}
.p-card.active-card{border-color:rgba(255,255,255,0.2)}

.card-top{
  display:flex;align-items:stretch;
  cursor:pointer;
  padding:14px 18px;gap:14px;
}
.card-top-left{flex:1;display:flex;flex-direction:column;gap:5px}
.card-badges{display:flex;gap:6px;flex-wrap:wrap;align-items:center}
.cat-badge{
  font-family:'Space Mono',monospace;font-size:9px;font-weight:700;
  letter-spacing:1px;padding:3px 9px;border-radius:20px;
  text-transform:uppercase;
}
.week-badge{
  font-size:10px;color:var(--dimmer);
  background:rgba(255,255,255,0.05);
  padding:3px 8px;border-radius:20px;
  border:1px solid var(--border);
}
.card-title{font-size:14px;font-weight:600;line-height:1.3;color:var(--white)}
.card-top-right{display:flex;align-items:center;gap:10px;flex-shrink:0}

/* status toggle */
.status-toggle{
  display:flex;flex-direction:column;align-items:center;gap:4px;
  cursor:pointer;padding:4px 8px;border-radius:10px;
  transition:background .15s;
  border:1px solid var(--border);background:var(--glass);
}
.status-toggle:hover{background:var(--navy4)}
.status-dot{width:12px;height:12px;border-radius:50%;transition:background .3s}
.status-txt{font-size:8px;font-family:'Space Mono',monospace;letter-spacing:0.3px;white-space:nowrap}

.expand-arrow{
  font-size:12px;color:var(--dimmer);
  transition:transform .25s;
  display:flex;align-items:center;padding:0 2px;
}
.p-card.open .expand-arrow{transform:rotate(180deg)}

/* card body (expandable) */
.card-body{
  display:none;border-top:1px solid var(--border);
}
.p-card.open .card-body{display:block}

.card-body-inner{padding:18px 18px 20px;display:grid;grid-template-columns:1fr 1fr;gap:20px}

/* topics col */
.topics-col h4{font-size:10px;font-family:'Space Mono',monospace;letter-spacing:1px;color:var(--dimmer);margin-bottom:10px;text-transform:uppercase}
.topic-item{
  display:flex;align-items:center;gap:8px;
  padding:6px 8px;border-radius:8px;
  font-size:12px;color:var(--dim);line-height:1.4;
  cursor:pointer;transition:background .15s;
  margin-bottom:3px;
}
.topic-item:hover{background:rgba(255,255,255,0.04)}
.topic-check{
  width:16px;height:16px;border-radius:4px;
  border:1.5px solid rgba(255,255,255,0.2);
  display:flex;align-items:center;justify-content:center;
  flex-shrink:0;transition:all .15s;font-size:9px;
}
.topic-item.done-topic .topic-check{background:var(--done);border-color:var(--done);color:#fff}
.topic-item.done-topic span{text-decoration:line-through;opacity:0.45}
.tools-row{display:flex;flex-wrap:wrap;gap:5px;margin-top:12px}
.tool-chip{
  font-family:'Space Mono',monospace;font-size:9px;font-weight:700;
  padding:3px 9px;border-radius:20px;border:1px solid;letter-spacing:0.3px;
}

/* updates col */
.updates-col h4{font-size:10px;font-family:'Space Mono',monospace;letter-spacing:1px;color:var(--dimmer);margin-bottom:10px;text-transform:uppercase}
.update-list{display:flex;flex-direction:column;gap:6px;margin-bottom:10px;max-height:180px;overflow-y:auto}
.update-list::-webkit-scrollbar{width:3px}
.update-list::-webkit-scrollbar-track{background:transparent}
.update-list::-webkit-scrollbar-thumb{background:var(--border2);border-radius:3px}
.update-entry{
  background:var(--navy3);border:1px solid var(--border);
  border-radius:8px;padding:8px 10px;
  display:flex;flex-direction:column;gap:2px;
}
.update-date{font-family:'Space Mono',monospace;font-size:9px;color:var(--dimmer);letter-spacing:0.3px}
.update-text{font-size:12px;color:rgba(255,255,255,0.75);line-height:1.4}
.update-del{
  align-self:flex-end;font-size:9px;color:var(--dimmer);
  cursor:pointer;padding:2px 6px;border-radius:4px;
  background:none;border:none;color:rgba(255,80,80,0.5);
  transition:color .15s;
}
.update-del:hover{color:var(--coral)}
.no-updates{font-size:12px;color:var(--dimmer);font-style:italic;padding:8px 0}

.update-input-row{display:flex;gap:6px}
.update-input{
  flex:1;background:var(--navy3);border:1px solid var(--border2);
  border-radius:8px;padding:7px 10px;
  font-size:12px;color:var(--white);font-family:'DM Sans',sans-serif;
  outline:none;transition:border-color .15s;resize:none;
}
.update-input:focus{border-color:rgba(255,255,255,0.3)}
.update-input::placeholder{color:var(--dimmer)}
.add-update-btn{
  background:var(--gold);color:var(--navy);
  border:none;border-radius:8px;padding:7px 12px;
  font-family:'Space Mono',monospace;font-size:10px;font-weight:700;
  cursor:pointer;white-space:nowrap;transition:opacity .15s;
}
.add-update-btn:hover{opacity:.85}

/* notes area */
.notes-area{
  padding:12px 18px 16px;
  border-top:1px solid var(--border);
  display:none;
}
.p-card.open .notes-area{display:block}
.notes-label{font-size:10px;font-family:'Space Mono',monospace;letter-spacing:1px;color:var(--dimmer);margin-bottom:6px;text-transform:uppercase}
.notes-input{
  width:100%;background:var(--navy3);border:1px solid var(--border2);
  border-radius:8px;padding:8px 12px;
  font-size:12px;color:rgba(255,255,255,0.75);font-family:'DM Sans',sans-serif;
  outline:none;transition:border-color .15s;resize:vertical;min-height:50px;
  line-height:1.5;
}
.notes-input:focus{border-color:rgba(255,255,255,0.3)}
.notes-input::placeholder{color:var(--dimmer)}

/* ── PICTOGRAPH STRIP ── */
.pict-strip{
  padding:8px 32px 0;
  display:flex;gap:0;
  border-top:1px solid var(--border);
  margin:0 32px;
}
.pict-cell{
  flex:1;display:flex;flex-direction:column;
  align-items:center;gap:4px;padding:12px 4px;
  border-right:1px solid var(--border);
}
.pict-cell:last-child{border-right:none}
.pict-ico{font-size:20px;line-height:1}
.pict-lbl{font-size:9px;color:var(--dimmer);text-align:center;line-height:1.3}

/* ── COLOR HELPERS ── */
.c-purple .node-circle{background:rgba(123,92,240,0.25);border-color:var(--purple);color:var(--purple-l)} .c-purple .spine-line{background:var(--purple)}
.c-teal   .node-circle{background:rgba(16,185,129,0.25);border-color:var(--teal);  color:var(--teal-l)}   .c-teal   .spine-line{background:var(--teal)}
.c-blue   .node-circle{background:rgba(59,130,246,0.25); border-color:var(--blue);  color:var(--blue-l)}   .c-blue   .spine-line{background:var(--blue)}
.c-gold   .node-circle{background:rgba(245,158,11,0.25); border-color:var(--gold);  color:var(--gold-l)}   .c-gold   .spine-line{background:var(--gold)}
.c-coral  .node-circle{background:rgba(239,68,68,0.25);  border-color:var(--coral); color:var(--coral-l)}  .c-coral  .spine-line{background:var(--coral)}
.c-green  .node-circle{background:rgba(34,197,94,0.25);  border-color:var(--green); color:var(--green-l)}  .c-green  .spine-line{background:var(--green)}

.cat-purple{background:rgba(123,92,240,0.15);color:#C4B5FD;border:1px solid rgba(123,92,240,0.3)}
.cat-teal  {background:rgba(16,185,129,0.15);color:#6EE7B7;border:1px solid rgba(16,185,129,0.3)}
.cat-blue  {background:rgba(59,130,246,0.15);color:#93C5FD;border:1px solid rgba(59,130,246,0.3)}
.cat-gold  {background:rgba(245,158,11,0.15);color:#FCD34D;border:1px solid rgba(245,158,11,0.3)}
.cat-coral {background:rgba(239,68,68,0.15); color:#FCA5A5;border:1px solid rgba(239,68,68,0.3)}
.cat-green {background:rgba(34,197,94,0.15); color:#86EFAC;border:1px solid rgba(34,197,94,0.3)}

.tool-purple{background:rgba(123,92,240,0.12);color:#C4B5FD;border-color:rgba(123,92,240,0.35)}
.tool-teal  {background:rgba(16,185,129,0.12);color:#6EE7B7;border-color:rgba(16,185,129,0.35)}
.tool-blue  {background:rgba(59,130,246,0.12);color:#93C5FD;border-color:rgba(59,130,246,0.35)}
.tool-gold  {background:rgba(245,158,11,0.12);color:#FCD34D;border-color:rgba(245,158,11,0.35)}
.tool-coral {background:rgba(239,68,68,0.12); color:#FCA5A5;border-color:rgba(239,68,68,0.35)}
.tool-green {background:rgba(34,197,94,0.12); color:#86EFAC;border-color:rgba(34,197,94,0.35)}

/* status colors */
.s-done   .status-dot{background:var(--done)}   .s-done   .status-txt{color:#86EFAC}
.s-active .status-dot{background:var(--inprog)}  .s-active .status-txt{color:#FCD34D}
.s-todo   .status-dot{background:rgba(255,255,255,0.2)} .s-todo .status-txt{color:var(--dimmer)}

/* completed phase card tint */
.phase-row.status-done .p-card{background:rgba(34,197,94,0.04);border-color:rgba(34,197,94,0.2)}
.phase-row.status-done .node-circle{box-shadow:0 0 14px rgba(34,197,94,0.3)}
.phase-row.status-active .node-circle{box-shadow:0 0 16px rgba(245,158,11,0.35)}

/* ── MODAL ── */
.modal-overlay{
  position:fixed;inset:0;background:rgba(0,0,0,0.7);
  backdrop-filter:blur(8px);z-index:500;
  display:flex;align-items:center;justify-content:center;
  opacity:0;pointer-events:none;transition:opacity .2s;
}
.modal-overlay.open{opacity:1;pointer-events:all}
.modal{
  background:var(--navy2);border:1px solid var(--border2);
  border-radius:20px;padding:28px 32px;
  width:400px;max-width:90vw;
  transform:translateY(12px);transition:transform .2s;
}
.modal-overlay.open .modal{transform:translateY(0)}
.modal h3{font-family:'Space Mono',monospace;font-size:14px;margin-bottom:16px;color:var(--white)}
.modal-note{font-size:12px;color:var(--dimmer);margin-bottom:20px;line-height:1.5}
.modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:20px}

/* ── EXPORT BUTTON ── */
.export-wrap{
  position:fixed;bottom:24px;right:24px;
  display:flex;flex-direction:column;gap:8px;align-items:flex-end;z-index:300;
}

/* ── TOAST ── */
.toast{
  position:fixed;bottom:80px;left:50%;transform:translateX(-50%) translateY(20px);
  background:var(--navy4);border:1px solid var(--border2);
  border-radius:30px;padding:10px 20px;
  font-size:12px;color:var(--white);font-family:'Space Mono',monospace;letter-spacing:0.5px;
  opacity:0;transition:all .3s;pointer-events:none;white-space:nowrap;z-index:400;
}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <div class="topbar-title">BEV-LIO <span>Project Tracker</span></div>
    <div class="topbar-sub" id="last-saved">No saves yet</div>
  </div>
  <div class="topbar-right">
    <button class="pill-btn" onclick="expandAll()">▼ Expand All</button>
    <button class="pill-btn" onclick="collapseAll()">▲ Collapse All</button>
    <button class="pill-btn" onclick="showResetModal()">⚠ Reset</button>
    <button class="pill-btn accent" onclick="exportPNG()">⬇ Export PNG</button>
  </div>
</div>

<!-- OVERVIEW -->
<div class="overview">
  <div class="ov-card">
    <div class="ov-num" id="ov-done" style="color:var(--green)">0</div>
    <div class="ov-lbl">Completed</div>
  </div>
  <div class="ov-card">
    <div class="ov-num" id="ov-active" style="color:var(--gold)">0</div>
    <div class="ov-lbl">In Progress</div>
  </div>
  <div class="ov-card">
    <div class="ov-num" id="ov-todo" style="color:var(--dimmer)">0</div>
    <div class="ov-lbl">Not Started</div>
  </div>
  <div class="ov-card">
    <div class="ov-num" id="ov-topics" style="color:var(--blue)">0</div>
    <div class="ov-lbl">Topics Done</div>
  </div>
  <div class="progress-bar-wrap">
    <div class="progress-bar-lbl">
      <span style="font-size:11px;font-weight:500;color:var(--white)">Overall Progress</span>
      <span id="pct-lbl">0%</span>
    </div>
    <div class="progress-bar-track"><div class="progress-bar-fill" id="prog-fill" style="width:0%"></div></div>
  </div>
</div>

<!-- TIMELINE -->
<div class="timeline-wrap" id="timeline"></div>

<!-- PICTOGRAPH STRIP -->
<div style="margin:0 32px">
  <div class="pict-strip" id="pict-strip"></div>
</div>

<!-- EXPORT + TOAST -->
<div class="export-wrap">
  <button class="pill-btn accent" style="box-shadow:0 4px 20px rgba(245,158,11,0.35)" onclick="exportPNG()">⬇ Export PNG</button>
</div>
<div class="toast" id="toast"></div>

<!-- RESET MODAL -->
<div class="modal-overlay" id="reset-modal">
  <div class="modal">
    <h3>Reset all progress?</h3>
    <div class="modal-note">This will clear all statuses, topic completions, updates, and notes. The phase structure will remain. This cannot be undone.</div>
    <div class="modal-actions">
      <button class="pill-btn" onclick="closeModal()">Cancel</button>
      <button class="pill-btn danger" onclick="resetAll()">Yes, reset everything</button>
    </div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script>
// ── DATA ──────────────────────────────────────────────────────────────────────
const PHASES = [
  {id:'p01',ico:'🧮',num:'01',color:'purple',cat:'Foundations',catCls:'cat-purple',toolCls:'tool-purple',spineLine:'spine-purple',
   title:'Mathematical & Robotics Foundations',week:'Weeks 1–2',
   topics:['Linear algebra — vectors, matrices, rotations','SE(3) transformations in 3D','Probability & Gaussian noise models','Bayesian estimation & Kalman Filter (EKF)','Optimization: Gauss-Newton, Levenberg-Marquardt'],
   tools:['GTSAM','Ceres Solver','Eigen']},
  {id:'p02',ico:'📐',num:'02',color:'purple',cat:'Foundations',catCls:'cat-purple',toolCls:'tool-purple',
   title:'Coordinate Frames & Sensor Calibration',week:'Weeks 2–3',
   topics:['LiDAR, IMU, camera & world frames','T_world = T_imu × T_lidar composition','Intrinsic calibration (focal length, distortion)','Extrinsic calibration between sensors','Calibration tools and workflow'],
   tools:['Kalibr','Autoware','ROS tf2']},
  {id:'p03',ico:'📡',num:'03',color:'teal',cat:'Sensors',catCls:'cat-teal',toolCls:'tool-teal',
   title:'LiDAR — Deep Understanding',week:'Week 3',
   topics:['Point cloud structure (x,y,z,intensity,t)','Scan patterns & ring IDs','Motion distortion causes & effects','Time of Flight: distance = c×t/2','Beam count, FoV & range specs'],
   tools:['PCL','Open3D','ROS sensor_msgs']},
  {id:'p04',ico:'⚡',num:'04',color:'teal',cat:'Sensors',catCls:'cat-teal',toolCls:'tool-teal',
   title:'IMU — Deep Understanding',week:'Week 4',
   topics:['Accelerometer & gyroscope axes (6-DOF)','Bias, noise & drift accumulation','100–1000 Hz update rate vs LiDAR 10 Hz','IMU pre-integration theory','Error-State Kalman Filter (ESKF)'],
   tools:['Eigen','ESKF','IMU simulator']},
  {id:'p05',ico:'🔍',num:'05',color:'blue',cat:'Algorithms',catCls:'cat-blue',toolCls:'tool-blue',
   title:'LiDAR Odometry & Scan Matching',week:'Week 5',
   topics:['Feature extraction: edges & planar points','ICP — Iterative Closest Point','NDT — Normal Distribution Transform','Pose representation (x,y,z,roll,pitch,yaw)','LOAM feature matching pipeline'],
   tools:['PCL','LOAM','ndt_omp']},
  {id:'p06',ico:'🧠',num:'06',color:'blue',cat:'Algorithms',catCls:'cat-blue',toolCls:'tool-blue',
   title:'LiDAR-Inertial Odometry (LIO)',week:'Week 6',
   topics:['IMU prediction → LiDAR correction loop','Scan de-skewing using IMU trajectory','Factor graph optimization','★ Study: FAST-LIO2 (Xu et al., 2022)','LIO-SAM full architecture walkthrough'],
   tools:['FAST-LIO2','LIO-SAM','GTSAM']},
  {id:'p07',ico:'🦅',num:'07',color:'gold',cat:'Mapping',catCls:'cat-gold',toolCls:'tool-gold',
   title:'BEV Map Generation',week:'Week 7',
   topics:['Project (x,y,z) → (x,y) ground plane','Occupancy grid creation','Height map & intensity channels','BEV output for path planning','Obstacle detection layer'],
   tools:['Open3D','ROS costmap','grid_map']},
  {id:'p08',ico:'🔀',num:'08',color:'coral',cat:'Fusion',catCls:'cat-coral',toolCls:'tool-coral',
   title:'Multi-Sensor Fusion',week:'Week 8',
   topics:['LiDAR→geometry, IMU→motion','Radar→velocity, Depth cam→close range','Low / feature / decision fusion levels','Sensor dropout graceful handling','Unified BEV output pipeline'],
   tools:['EKF','Factor Graph','ROS sync']},
  {id:'p09',ico:'🔄',num:'09',color:'blue',cat:'Algorithms',catCls:'cat-blue',toolCls:'tool-blue',
   title:'SLAM & Loop Closure',week:'Weeks 8–9',
   topics:['Loop closure detection strategy','Scan Context descriptor','Pose graph optimization after closure','Point cloud & BEV semantic map types','Deep study: LIO-SAM loop closure'],
   tools:['LIO-SAM','GTSAM','ScanContext']},
  {id:'p10',ico:'🤖',num:'10',color:'green',cat:'System',catCls:'cat-green',toolCls:'tool-green',
   title:'ROS2 Software Stack',week:'Throughout',
   topics:['Topics, nodes, services, actions','Transform tree (tf2) management','RViz point cloud visualization','Gazebo simulation & testing','rosbag recording & replay'],
   tools:['ROS2','RViz','Gazebo','rosbag2']},
  {id:'p11',ico:'🚀',num:'11',color:'green',cat:'System',catCls:'cat-green',toolCls:'tool-green',
   title:'Full System Integration & Testing',week:'Week 9+',
   topics:['Time synchronization all sensors','End-to-end pipeline assembly','APE / RPE trajectory accuracy metrics','Test on KITTI & NuScenes datasets','Tune params, benchmark, write-up'],
   tools:['KITTI','NuScenes','evo (eval)']}
];

const PICTS = [
  {ico:'∑',lbl:'Linear Algebra'},{ico:'🎯',lbl:'Calibration'},{ico:'☁️',lbl:'Point Cloud'},
  {ico:'📈',lbl:'IMU Fusion'},{ico:'⚙️',lbl:'Scan Match'},{ico:'🧠',lbl:'LIO Core'},
  {ico:'🦅',lbl:"Bird's Eye View"},{ico:'🌐',lbl:'4-Sensor'},{ico:'♾️',lbl:'Loop Closure'},
  {ico:'📦',lbl:'ROS2 Stack'},{ico:'✅',lbl:'Live System'}
];

const STATUSES = ['todo','active','done'];
const STATUS_LABELS = {todo:'To Do',active:'In Progress',done:'Done'};

// ── STATE ──────────────────────────────────────────────────────────────────────
let state = {};

function defaultState() {
  const s = {};
  PHASES.forEach(p => {
    s[p.id] = {
      status: 'todo',
      topics: Array(p.topics.length).fill(false),
      updates: [],
      notes: ''
    };
  });
  return s;
}

function loadState() {
  try {
    const raw = localStorage.getItem('bevlio_tracker_v2');
    if (raw) {
      state = JSON.parse(raw);
      // backfill missing
      PHASES.forEach(p => {
        if (!state[p.id]) state[p.id] = {status:'todo',topics:Array(p.topics.length).fill(false),updates:[],notes:''};
        if (!state[p.id].updates) state[p.id].updates = [];
        if (!state[p.id].notes) state[p.id].notes = '';
      });
    } else {
      state = defaultState();
    }
  } catch(e) { state = defaultState(); }
}

function saveState() {
  localStorage.setItem('bevlio_tracker_v2', JSON.stringify(state));
  const now = new Date();
  document.getElementById('last-saved').textContent =
    'Saved ' + now.toLocaleDateString() + ' ' + now.toLocaleTimeString([], {hour:'2-digit',minute:'2-digit'});
}

// ── RENDER ────────────────────────────────────────────────────────────────────
function renderTimeline() {
  const wrap = document.getElementById('timeline');
  wrap.innerHTML = '';

  PHASES.forEach((p, idx) => {
    const s = state[p.id];
    const isLast = idx === PHASES.length - 1;

    const row = document.createElement('div');
    row.className = `phase-row c-${p.color} status-${s.status}`;
    row.id = `row-${p.id}`;

    // spine
    const spine = document.createElement('div');
    spine.className = 'spine';
    spine.innerHTML = `
      <div class="node-circle" onclick="cycleStatus('${p.id}')" title="Click to cycle status">
        <span class="node-ico">${p.ico}</span>
        <span class="node-num">${p.num}</span>
      </div>
      ${!isLast ? `<div class="spine-line"></div>` : ''}
    `;

    // card
    const card = document.createElement('div');
    card.className = 'p-card';
    card.id = `card-${p.id}`;

    const doneTops = s.topics.filter(Boolean).length;
    const totalTops = p.topics.length;
    const pct = totalTops ? Math.round(doneTops/totalTops*100) : 0;

    card.innerHTML = `
      <div class="card-top" onclick="toggleCard('${p.id}')">
        <div class="card-top-left">
          <div class="card-badges">
            <span class="cat-badge ${p.catCls}">${p.cat}</span>
            <span class="week-badge">${p.week}</span>
            ${s.updates.length ? `<span class="week-badge" style="color:var(--gold);border-color:rgba(245,158,11,0.3);background:rgba(245,158,11,0.08)">📝 ${s.updates.length} update${s.updates.length>1?'s':''}</span>` : ''}
            ${doneTops>0 ? `<span class="week-badge" style="color:var(--blue-l);border-color:rgba(59,130,246,0.3);background:rgba(59,130,246,0.08)">${doneTops}/${totalTops} topics</span>` : ''}
          </div>
          <div class="card-title">${p.title}</div>
        </div>
        <div class="card-top-right">
          <div class="status-toggle s-${s.status}" onclick="event.stopPropagation();cycleStatus('${p.id}')" title="Click to change status">
            <div class="status-dot"></div>
            <div class="status-txt">${STATUS_LABELS[s.status]}</div>
          </div>
          <div class="expand-arrow">▼</div>
        </div>
      </div>

      <div class="card-body">
        <div class="card-body-inner">
          <div class="topics-col">
            <h4>Topics to cover</h4>
            <div class="topic-list" id="topics-${p.id}">
              ${p.topics.map((t,i) => `
                <div class="topic-item ${s.topics[i]?'done-topic':''}" onclick="toggleTopic('${p.id}',${i})">
                  <div class="topic-check">${s.topics[i]?'✓':''}</div>
                  <span>${t}</span>
                </div>`).join('')}
            </div>
            <div class="tools-row">
              ${p.tools.map(t => `<span class="tool-chip ${p.toolCls}">${t}</span>`).join('')}
            </div>
          </div>

          <div class="updates-col">
            <h4>Progress updates</h4>
            <div class="update-list" id="updates-${p.id}">
              ${renderUpdates(p.id)}
            </div>
            <div class="update-input-row">
              <textarea class="update-input" id="update-input-${p.id}" placeholder="Add an update, note, or milestone…" rows="2" onkeydown="handleUpdateKey(event,'${p.id}')"></textarea>
              <button class="add-update-btn" onclick="addUpdate('${p.id}')">Add</button>
            </div>
          </div>
        </div>
      </div>

      <div class="notes-area">
        <div class="notes-label">Personal notes</div>
        <textarea class="notes-input" id="notes-${p.id}" placeholder="Jot down anything — paper references, questions, observations…" onblur="saveNotes('${p.id}')">${s.notes||''}</textarea>
      </div>
    `;

    const content = document.createElement('div');
    content.className = 'phase-content';
    content.appendChild(card);

    row.appendChild(spine);
    row.appendChild(content);
    wrap.appendChild(row);
  });

  // pictograph
  const pict = document.getElementById('pict-strip');
  pict.innerHTML = PICTS.map(p => `
    <div class="pict-cell">
      <span class="pict-ico">${p.ico}</span>
      <span class="pict-lbl">${p.lbl}</span>
    </div>`).join('');

  updateOverview();
}

function renderUpdates(pid) {
  const updates = state[pid].updates;
  if (!updates.length) return `<div class="no-updates">No updates yet.</div>`;
  return updates.slice().reverse().map((u,ri) => {
    const i = updates.length - 1 - ri;
    return `
    <div class="update-entry">
      <div style="display:flex;justify-content:space-between;align-items:flex-start">
        <div class="update-date">${u.date}</div>
        <button class="update-del" onclick="deleteUpdate('${pid}',${i})">✕</button>
      </div>
      <div class="update-text">${escHtml(u.text)}</div>
    </div>`;
  }).join('');
}

function escHtml(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

// ── INTERACTIONS ──────────────────────────────────────────────────────────────
function toggleCard(pid) {
  const card = document.getElementById(`card-${pid}`);
  card.classList.toggle('open');
}

function expandAll() {
  document.querySelectorAll('.p-card').forEach(c => c.classList.add('open'));
}
function collapseAll() {
  document.querySelectorAll('.p-card').forEach(c => c.classList.remove('open'));
}

function cycleStatus(pid) {
  const cur = state[pid].status;
  const idx = STATUSES.indexOf(cur);
  state[pid].status = STATUSES[(idx+1) % STATUSES.length];
  saveState();
  rerenderRow(pid);
  updateOverview();
  toast(state[pid].status==='done'?'Phase marked done ✓':state[pid].status==='active'?'Phase in progress…':'Phase reset to To Do');
}

function toggleTopic(pid, idx) {
  state[pid].topics[idx] = !state[pid].topics[idx];
  saveState();
  // update just the topic item
  const items = document.querySelectorAll(`#topics-${pid} .topic-item`);
  items[idx].className = `topic-item${state[pid].topics[idx]?' done-topic':''}`;
  items[idx].querySelector('.topic-check').textContent = state[pid].topics[idx]?'✓':'';
  updateOverview();
  refreshBadges(pid);
}

function addUpdate(pid) {
  const inp = document.getElementById(`update-input-${pid}`);
  const text = inp.value.trim();
  if (!text) return;
  const now = new Date();
  const date = now.toLocaleDateString('en-GB',{day:'numeric',month:'short',year:'numeric'}) + ' · ' +
    now.toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'});
  state[pid].updates.push({date, text});
  inp.value = '';
  saveState();
  document.getElementById(`updates-${pid}`).innerHTML = renderUpdates(pid);
  refreshBadges(pid);
  toast('Update saved!');
}

function handleUpdateKey(e, pid) {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); addUpdate(pid); }
}

function deleteUpdate(pid, idx) {
  state[pid].updates.splice(idx, 1);
  saveState();
  document.getElementById(`updates-${pid}`).innerHTML = renderUpdates(pid);
  refreshBadges(pid);
}

function saveNotes(pid) {
  const el = document.getElementById(`notes-${pid}`);
  if (el) { state[pid].notes = el.value; saveState(); }
}

function refreshBadges(pid) {
  // re-render just the badge row without full rerender
  const s = state[pid];
  const p = PHASES.find(x=>x.id===pid);
  const doneTops = s.topics.filter(Boolean).length;
  const totalTops = p.topics.length;
  const badgeRow = document.querySelector(`#card-${pid} .card-badges`);
  if (!badgeRow) return;
  badgeRow.innerHTML = `
    <span class="cat-badge ${p.catCls}">${p.cat}</span>
    <span class="week-badge">${p.week}</span>
    ${s.updates.length ? `<span class="week-badge" style="color:var(--gold);border-color:rgba(245,158,11,0.3);background:rgba(245,158,11,0.08)">📝 ${s.updates.length} update${s.updates.length>1?'s':''}</span>` : ''}
    ${doneTops>0 ? `<span class="week-badge" style="color:var(--blue-l);border-color:rgba(59,130,246,0.3);background:rgba(59,130,246,0.08)">${doneTops}/${totalTops} topics</span>` : ''}
  `;
}

function rerenderRow(pid) {
  const s = state[pid];
  const row = document.getElementById(`row-${pid}`);
  if (!row) return;
  row.className = `phase-row c-${PHASES.find(p=>p.id===pid).color} status-${s.status}`;
  const stoggle = row.querySelector('.status-toggle');
  if (stoggle) {
    stoggle.className = `status-toggle s-${s.status}`;
    stoggle.querySelector('.status-txt').textContent = STATUS_LABELS[s.status];
  }
}

function updateOverview() {
  let done=0,active=0,todo=0,topicsDone=0,topicsTotal=0;
  PHASES.forEach(p => {
    const s = state[p.id];
    if (s.status==='done') done++;
    else if (s.status==='active') active++;
    else todo++;
    topicsDone += s.topics.filter(Boolean).length;
    topicsTotal += p.topics.length;
  });
  document.getElementById('ov-done').textContent = done;
  document.getElementById('ov-active').textContent = active;
  document.getElementById('ov-todo').textContent = todo;
  document.getElementById('ov-topics').textContent = topicsDone;
  const pct = topicsTotal ? Math.round(topicsDone/topicsTotal*100) : 0;
  document.getElementById('pct-lbl').textContent = pct + '%';
  document.getElementById('prog-fill').style.width = pct + '%';
}

// ── RESET ──────────────────────────────────────────────────────────────────────
function showResetModal() { document.getElementById('reset-modal').classList.add('open'); }
function closeModal() { document.getElementById('reset-modal').classList.remove('open'); }
function resetAll() {
  state = defaultState();
  saveState();
  closeModal();
  renderTimeline();
  toast('All progress reset.');
}

// ── EXPORT ────────────────────────────────────────────────────────────────────
async function exportPNG() {
  const btns = document.querySelectorAll('.pill-btn, .export-wrap, .topbar, .toast, .modal-overlay');
  toast('Rendering export…');
  const el = document.body;
  try {
    const canvas = await html2canvas(el, {
      scale: 1.5,
      backgroundColor: '#09111F',
      useCORS: true,
      logging: false,
      ignoreElements: el => el.classList && (el.classList.contains('export-wrap') || el.classList.contains('modal-overlay') || el.classList.contains('toast'))
    });
    const link = document.createElement('a');
    link.download = 'BEV_LIO_Roadmap_' + new Date().toISOString().slice(0,10) + '.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
    toast('PNG exported!');
  } catch(e) { toast('Export failed: '+e.message); }
}

// ── TOAST ──────────────────────────────────────────────────────────────────────
let toastTimer;
function toast(msg) {
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => el.classList.remove('show'), 2200);
}

// ── INIT ──────────────────────────────────────────────────────────────────────
loadState();
renderTimeline();

// close modal on backdrop click
document.getElementById('reset-modal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});
</script>
</body>
</html>
