import { useState, useRef } from "react";

const soilTypes = [
  { id:"sandy", name:"Sandy Soil", emoji:"🏖️", color:"#D4900A", bg:"#FFFBEA", border:"#E6B800",
    texture:"Coarse & gritty", drainage:"Excellent (too fast)", fertility:"Low", waterRetention:"Very Low",
    pHRange:"5.5 – 7.0", organic:"Low", bestFor:["Root vegetables","Cacti","Succulents","Lavender"],
    description:"Sandy soil has large particles with lots of air pockets. Water and nutrients drain quickly, making it challenging for most crops but ideal for drought-tolerant plants.",
    improvements:["Add compost or organic matter","Mulch to retain moisture","Use drip irrigation","Add perlite for structure"],
    characteristics:{ sand:90, clay:5, silt:5 } },
  { id:"clay", name:"Clay Soil", emoji:"🧱", color:"#A0522D", bg:"#FFF0E8", border:"#A0522D",
    texture:"Dense & sticky", drainage:"Poor (waterlogged)", fertility:"High", waterRetention:"Very High",
    pHRange:"6.0 – 7.5", organic:"Moderate", bestFor:["Wheat","Rice","Cabbage","Broccoli"],
    description:"Clay soil has tiny particles that pack tightly together. It holds nutrients well but can become waterlogged and is very hard when dry.",
    improvements:["Add gypsum to break up clay","Mix in coarse sand","Add organic compost","Avoid working when wet"],
    characteristics:{ sand:10, clay:70, silt:20 } },
  { id:"loamy", name:"Loamy Soil", emoji:"🌱", color:"#2E7D32", bg:"#F0FFF0", border:"#2E7D32",
    texture:"Crumbly & balanced", drainage:"Good (balanced)", fertility:"Very High", waterRetention:"Good",
    pHRange:"6.0 – 7.0", organic:"High", bestFor:["Almost all crops","Vegetables","Fruits","Flowers"],
    description:"Loamy soil is the ideal garden soil — a perfect mix of sand, silt, and clay. It drains well yet retains moisture and nutrients, making it excellent for growing almost anything.",
    improvements:["Maintain with compost annually","Avoid compaction","Regular mulching","Test pH every 2 years"],
    characteristics:{ sand:40, clay:20, silt:40 } },
  { id:"silty", name:"Silty Soil", emoji:"💧", color:"#3949AB", bg:"#EEF2FF", border:"#3949AB",
    texture:"Smooth & slippery", drainage:"Moderate", fertility:"Moderate-High", waterRetention:"High",
    pHRange:"6.0 – 7.0", organic:"Moderate-High", bestFor:["Wetland plants","Shrubs","Grass","Climbers"],
    description:"Silty soil feels smooth and silky when wet. It is fertile and retains moisture well but can compact easily, especially near rivers and flood plains.",
    improvements:["Add organic matter to improve drainage","Avoid heavy machinery","Plant cover crops","Add coarse materials for aeration"],
    characteristics:{ sand:20, clay:10, silt:70 } },
  { id:"peaty", name:"Peaty Soil", emoji:"🌿", color:"#5D4037", bg:"#FBF4EF", border:"#4E342E",
    texture:"Spongy & dark", drainage:"Variable", fertility:"Low (acidic)", waterRetention:"Extremely High",
    pHRange:"3.5 – 5.5", organic:"Very High", bestFor:["Blueberries","Heather","Azaleas","Rhododendrons"],
    description:"Peaty soil forms from partially decomposed organic matter in boggy areas. It is highly acidic, retains lots of water, and warms slowly in spring.",
    improvements:["Add lime to raise pH","Mix in sand for drainage","Add fertilizers (naturally low in nutrients)","Allow to dry before planting"],
    characteristics:{ sand:10, clay:5, silt:85 } },
  { id:"chalky", name:"Chalky Soil", emoji:"🪨", color:"#616161", bg:"#FAFAFA", border:"#757575",
    texture:"Stony & pale", drainage:"Very Fast", fertility:"Low-Moderate", waterRetention:"Low",
    pHRange:"7.5 – 8.5", organic:"Low", bestFor:["Lavender","Lilac","Brassicas","Spinach"],
    description:"Chalky soil sits on chalk or limestone. It is alkaline, free-draining, and can cause yellowing (chlorosis) in acid-loving plants.",
    improvements:["Add sulfur to lower pH","Incorporate organic matter","Frequent irrigation needed","Choose alkaline-tolerant plants"],
    characteristics:{ sand:60, clay:10, silt:30 } },
];

const testingMethods = [
  { name:"Jar Test", icon:"🧪", difficulty:"Easy", cost:"Free", color:"#4CAF50",
    result:"See soil composition layers visually",
    steps:["Fill a glass jar 1/3 with soil","Add water and 1 tsp dish soap","Shake well and let settle for 24 hours","Sand settles first (bottom), then silt, then clay (top)"] },
  { name:"Ribbon Test", icon:"🎀", difficulty:"Easy", cost:"Free", color:"#9C27B0",
    result:">5 cm ribbon = clay-heavy; <2.5 cm = sandy",
    steps:["Take a handful of moist soil","Squeeze it into a ball","Push between thumb and finger into a ribbon","Measure ribbon length before it breaks"] },
  { name:"pH Test Kit", icon:"🔬", difficulty:"Moderate", cost:"₹150–₹600", color:"#F44336",
    result:"Precise soil acidity / alkalinity reading",
    steps:["Mix soil sample with distilled water","Dip pH test strip into mixture","Wait 30 seconds","Compare colour to chart provided"] },
  { name:"Percolation Test", icon:"💦", difficulty:"Easy", cost:"Free", color:"#2196F3",
    result:">4 hr drainage issue; <15 min = too sandy",
    steps:["Dig a hole 30 cm deep and 30 cm wide","Fill with water, let drain completely","Fill again and time how fast it drains","Record time for all water to disappear"] },
  { name:"Nutrient Test Kit", icon:"🌿", difficulty:"Moderate", cost:"₹300–₹1200", color:"#FF9800",
    result:"N, P, K nutrient levels",
    steps:["Collect soil from 15 cm depth","Mix with test solution per kit instructions","Add chemical reagent drops","Match colour to nutrient chart"] },
  { name:"Lab Analysis", icon:"🏛️", difficulty:"Expert", cost:"₹800–₹3000", color:"#607D8B",
    result:"Complete soil profile with amendment advice",
    steps:["Collect samples from 5+ spots in your plot","Mix and send 500 g to soil testing lab","Wait 1–2 weeks for results","Receive full report with recommendations"] },
];

const mlAlgorithms = [
  { name:"CNN (Deep Learning)", accuracy:96, color:"#9C27B0" },
  { name:"Random Forest", accuracy:94, color:"#4CAF50" },
  { name:"SVM", accuracy:89, color:"#2196F3" },
  { name:"KNN", accuracy:82, color:"#FF9800" },
  { name:"Decision Tree", accuracy:79, color:"#F44336" },
];

const teamMembers = [
  { name:"GL Pratham", usn:"4MH23CV016", role:"Team Leader (GL)", emoji:"🏆", color:"#E65100", bg:"#FFF8E1", border:"#FFB300", isLeader:true },
  { name:"Bharathi S", usn:"4MH23CV009", role:"Research Analyst", emoji:"🔬", color:"#1565C0", bg:"#E3F2FD", border:"#1565C0" },
  { name:"Poorvika", usn:"4MH24CV408", role:"Data Collection", emoji:"📊", color:"#6A1B9A", bg:"#F3E5F5", border:"#6A1B9A" },
  { name:"Vinod S", usn:"4MH24CV417", role:"Documentation", emoji:"📝", color:"#1B5E20", bg:"#E8F5E9", border:"#1B5E20" },
];

const worldRecords = [
  {
    title:"Nobel Book of World Record",
    org:"International Noble World Records (INWR)",
    recordNo:"IN24-103-423",
    date:"15 September 2025",
    achievement:"GPL Platinum Karnataka — Exclusive World Record Certificate",
    category:"World Record",
    gradA:"#B8860B", gradB:"#FFD700",
    icon:"🏅",
  },
  {
    title:"Largest Road Art Portrait for Engineers",
    org:"International Noble World Records (INWR)",
    recordNo:"IN24-103-423",
    date:"15 September 2025",
    achievement:"World's Largest Jio Portrait — Sir M. Visvesvaraya Portrait, MITM Sports Ground, Mysuru",
    category:"Exclusive World Record",
    gradA:"#1565C0", gradB:"#42A5F5",
    icon:"🎨",
  },
  {
    title:"Ingenious Jump World Record",
    org:"International Noble World Records (INWR)",
    recordNo:"IN24-103-423",
    date:"15 September 2025",
    achievement:"Certificate of Participation — Largest Road Art Portrait for Engineers Today",
    category:"Certificate of Participation",
    gradA:"#2E7D32", gradB:"#66BB6A",
    icon:"🌟",
  },
  {
    title:"Kings World Record",
    org:"Kings World Records",
    recordNo:"KWR-2025",
    date:"2025",
    achievement:"Certificate of Participation — Outstanding contribution to large-scale record-setting events, Karnataka",
    category:"Kings World Record",
    gradA:"#880E4F", gradB:"#E91E63",
    icon:"👑",
  },
];

function PrathamModal({ onClose }) {
  return (
    <div onClick={onClose} style={{
      position:"fixed", inset:0, background:"rgba(0,0,0,0.72)", zIndex:1000,
      display:"flex", alignItems:"flex-start", justifyContent:"center",
      overflowY:"auto", padding:"2rem 1rem",
    }}>
      <div onClick={e => e.stopPropagation()} style={{
        background:"white", borderRadius:24, maxWidth:680, width:"100%",
        overflow:"hidden", boxShadow:"0 24px 80px rgba(0,0,0,0.45)",
        margin:"auto",
      }}>
        {/* Header */}
        <div style={{ background:"linear-gradient(135deg,#E65100,#FF9800,#FFB300)", padding:"2rem", color:"white", position:"relative" }}>
          <button onClick={onClose} style={{ position:"absolute", top:14, right:14, background:"rgba(255,255,255,0.2)", border:"none", color:"white", borderRadius:"50%", width:34, height:34, fontSize:18, cursor:"pointer" }}>✕</button>
          <div style={{ display:"flex", gap:"1.25rem", alignItems:"center", flexWrap:"wrap" }}>
            <div style={{ width:76, height:76, borderRadius:"50%", background:"rgba(255,255,255,0.22)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:38, border:"3px solid white", flexShrink:0 }}>🏆</div>
            <div>
              <div style={{ fontSize:24, fontWeight:900, letterSpacing:"-0.5px" }}>GL Pratham</div>
              <div style={{ opacity:0.88, fontSize:13, marginTop:2 }}>USN: 4MH23CV016 · Team Leader (GL)</div>
              <div style={{ opacity:0.85, fontSize:13 }}>B.E. Civil Engineering · MIT Mysuru (MITM)</div>
              <div style={{ display:"flex", gap:6, marginTop:8, flexWrap:"wrap" }}>
                {["🏅 World Record Holder","🎥 YouTuber · Karnataka","📐 Civil Engineer","🌱 Soil Researcher"].map(t => (
                  <span key={t} style={{ background:"rgba(255,255,255,0.2)", borderRadius:20, padding:"3px 10px", fontSize:11, fontWeight:700 }}>{t}</span>
                ))}
              </div>
            </div>
          </div>
        </div>

        <div style={{ padding:"1.5rem" }}>
          {/* About */}
          <div style={{ marginBottom:"1.25rem" }}>
            <h3 style={{ fontSize:15, fontWeight:800, color:"#E65100", margin:"0 0 0.5rem" }}>👤 About</h3>
            <p style={{ color:"#555", lineHeight:1.75, fontSize:14, margin:0 }}>
              GL Pratham is a Civil Engineering student at <strong>Maharaja Institute of Technology Mysuru (MITM)</strong>, Mysore, Karnataka.
              He leads this soil science project, combining geotechnical engineering with AI and machine learning.
              Beyond academics, he is a content creator from Karnataka on YouTube — running the channel <em>Platinum Gorilla Triple 001</em>,
              producing vlogs, informative content, engineering topics, and science-related videos.
              He is also a multi-record holder, having set world records at the national level in 2025.
            </p>
          </div>

          {/* YouTube */}
          <div style={{ background:"#FFF3E0", borderRadius:14, padding:"1rem 1.25rem", marginBottom:"1.25rem", border:"2px solid #FFB300", display:"flex", gap:12, alignItems:"center" }}>
            <div style={{ fontSize:34 }}>▶️</div>
            <div style={{ flex:1 }}>
              <div style={{ fontWeight:800, color:"#BF360C", fontSize:15 }}>Platinum Gorilla Triple 001</div>
              <div style={{ fontSize:13, color:"#777", marginTop:2 }}>YouTube Channel · Karnataka, India</div>
              <div style={{ fontSize:12, color:"#888" }}>Vlogs · Informative Content · Engineering · Science</div>
            </div>
            <a href="https://www.youtube.com/@PlatinumGorillaTriple001" target="_blank" rel="noopener noreferrer"
              style={{ background:"#FF0000", color:"white", borderRadius:20, padding:"6px 16px", fontSize:12, fontWeight:700, textDecoration:"none", flexShrink:0 }}>
              🔗 Visit
            </a>
          </div>

          {/* World Records */}
          <h3 style={{ fontSize:15, fontWeight:800, color:"#333", margin:"0 0 0.75rem" }}>🏅 World Records & Certifications</h3>
          <div style={{ display:"flex", flexDirection:"column", gap:12, marginBottom:"1.25rem" }}>
            {worldRecords.map((r, i) => (
              <div key={i} style={{ borderRadius:14, overflow:"hidden", border:"1.5px solid rgba(0,0,0,0.08)", boxShadow:"0 3px 12px rgba(0,0,0,0.07)" }}>
                {/* Ribbon */}
                <div style={{ background:`linear-gradient(135deg,${r.gradA},${r.gradB},${r.gradA})`, padding:"9px 14px", display:"flex", alignItems:"center", gap:10 }}>
                  <span style={{ fontSize:22 }}>{r.icon}</span>
                  <div>
                    <div style={{ color:"white", fontWeight:900, fontSize:13, textShadow:"0 1px 3px rgba(0,0,0,0.25)" }}>{r.title}</div>
                    <div style={{ color:"rgba(255,255,255,0.82)", fontSize:11, fontWeight:600 }}>{r.category}</div>
                  </div>
                </div>
                {/* Body */}
                <div style={{ background:"#FFFEF8", padding:"9px 14px", borderTop:"2px dashed rgba(0,0,0,0.07)" }}>
                  <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr 1fr", gap:6, marginBottom:7 }}>
                    {[["Organisation",r.org],["Record No.",r.recordNo],["Date",r.date]].map(([k,v]) => (
                      <div key={k}>
                        <div style={{ fontSize:9, color:"#aaa", fontWeight:700, textTransform:"uppercase", marginBottom:1 }}>{k}</div>
                        <div style={{ fontSize:11, color:"#333", fontWeight:600, lineHeight:1.4 }}>{v}</div>
                      </div>
                    ))}
                  </div>
                  <div style={{ background:"white", borderRadius:8, padding:"6px 10px", border:"1px solid rgba(0,0,0,0.06)" }}>
                    <div style={{ fontSize:9, color:"#aaa", fontWeight:700, textTransform:"uppercase", marginBottom:2 }}>Achievement</div>
                    <div style={{ fontSize:12, color:"#444", lineHeight:1.5 }}>{r.achievement}</div>
                  </div>
                </div>
              </div>
            ))}
          </div>

          {/* Academic */}
          <div style={{ background:"#E8F5E9", borderRadius:14, padding:"1rem 1.25rem", border:"2px solid #4CAF50" }}>
            <h3 style={{ fontSize:14, fontWeight:800, color:"#2E7D32", margin:"0 0 0.5rem" }}>🎓 Academic Details</h3>
            {[
              ["🏫 Institution","Maharaja Institute of Technology Mysuru (MITM)"],
              ["📐 Department","Civil Engineering"],
              ["🔖 USN","4MH23CV016"],
              ["👨‍🏫 Project Guide","Prof. Murul Gyes (Varun Sir) — Associate Professor, Dept. of Civil Engineering, MIT Mysuru"],
            ].map(([k,v]) => (
              <div key={k} style={{ fontSize:13, color:"#555", lineHeight:2 }}><strong>{k}:</strong> {v}</div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
}

function PublishGuide() {
  const [step, setStep] = useState(0);
  const steps = [
    { icon:"👤", title:"Create a GitHub account", desc:'Go to github.com → Sign Up. Use your email. Suggested username: Mr-GL-Pratham', url:"https://github.com/signup", action:"Open GitHub" },
    { icon:"📁", title:'Create a new repository', desc:'After login click "+" → "New repository". Name it EXACTLY: mr-gl-pratham.github.io — this becomes your free website address!', url:"https://github.com/new", action:"Create repo" },
    { icon:"⚙️", title:"Configure repository", desc:'Set visibility to Public. Tick "Add a README file". Click "Create repository". Your future site: mr-gl-pratham.github.io', url:null },
    { icon:"📤", title:"Upload your file", desc:'Inside the repo click "Add file" → "Upload files". Rename soil_science_website.jsx to index.html first (ask me to convert it!). Commit the file.', url:null },
    { icon:"🌐", title:"Enable GitHub Pages", desc:'Go to Settings → Pages → Source: "Deploy from a branch" → Branch: main → /root → Save. Wait 2–3 minutes.', url:null },
    { icon:"🚀", title:"Your website is live!", desc:'Visit: https://mr-gl-pratham.github.io — Share this link with anyone, it is 100% free and public forever. No login needed to view.', url:"https://mr-gl-pratham.github.io", action:"Visit site" },
  ];
  return (
    <div style={{ background:"white", borderRadius:20, padding:"1.5rem", boxShadow:"0 2px 12px rgba(0,0,0,0.07)", marginTop:"1.25rem" }}>
      <h2 style={{ fontSize:19, fontWeight:800, color:"#1a237e", margin:"0 0 1rem" }}>🚀 Publish on GitHub Pages — Step by Step</h2>
      <div style={{ display:"flex", gap:6, marginBottom:"1.25rem", overflowX:"auto", paddingBottom:4 }}>
        {steps.map((s,i) => (
          <button key={i} onClick={() => setStep(i)} style={{
            background:step===i?"#1a237e":"#f0f0f0", color:step===i?"white":"#666",
            border:"none", borderRadius:20, padding:"5px 13px", fontSize:12, fontWeight:700,
            cursor:"pointer", whiteSpace:"nowrap", flexShrink:0,
          }}>Step {i+1}</button>
        ))}
      </div>
      <div style={{ background:"#E8EAF6", borderRadius:14, padding:"1.25rem", border:"2px solid #3949AB" }}>
        <div style={{ fontSize:30, marginBottom:6 }}>{steps[step].icon}</div>
        <div style={{ fontWeight:800, fontSize:16, color:"#1a237e", marginBottom:6 }}>{steps[step].title}</div>
        <div style={{ fontSize:14, color:"#555", lineHeight:1.7, marginBottom: steps[step].url?12:0 }}>{steps[step].desc}</div>
        {steps[step].url && (
          <a href={steps[step].url} target="_blank" rel="noopener noreferrer"
            style={{ display:"inline-block", background:"#1a237e", color:"white", borderRadius:20, padding:"6px 18px", fontSize:13, fontWeight:700, textDecoration:"none" }}>
            {steps[step].action} →
          </a>
        )}
      </div>
      <div style={{ display:"flex", justifyContent:"space-between", marginTop:"0.75rem" }}>
        <button disabled={step===0} onClick={() => setStep(s=>s-1)} style={{ background:step===0?"#eee":"#1a237e", color:step===0?"#bbb":"white", border:"none", borderRadius:20, padding:"6px 16px", cursor:step===0?"not-allowed":"pointer", fontSize:13, fontWeight:700 }}>← Prev</button>
        <span style={{ fontSize:12, color:"#888", alignSelf:"center" }}>{step+1} / {steps.length}</span>
        <button disabled={step===steps.length-1} onClick={() => setStep(s=>s+1)} style={{ background:step===steps.length-1?"#eee":"#1a237e", color:step===steps.length-1?"#bbb":"white", border:"none", borderRadius:20, padding:"6px 16px", cursor:step===steps.length-1?"not-allowed":"pointer", fontSize:13, fontWeight:700 }}>Next →</button>
      </div>
    </div>
  );
}

export default function SoilSenseApp() {
  const [section, setSection] = useState("home");
  const [selectedSoil, setSelectedSoil] = useState(null);
  const [expandedTest, setExpandedTest] = useState(null);
  const [showPratham, setShowPratham] = useState(false);
  const [showPublish, setShowPublish] = useState(false);
  const [uploadedImage, setUploadedImage] = useState(null);
  const [analyzing, setAnalyzing] = useState(false);
  const [aiResult, setAiResult] = useState(null);
  const [aiError, setAiError] = useState(null);
  const fileRef = useRef();

  const nav = [
    { id:"home", label:"🏠 Home" },
    { id:"soils", label:"🌍 Soil Types" },
    { id:"identify", label:"🔬 AI Identify" },
    { id:"testing", label:"🧪 Testing" },
    { id:"research", label:"📊 Research" },
    { id:"team", label:"👥 Team" },
  ];

  const handleFile = (file) => {
    if (!file || !file.type.startsWith("image/")) { setAiError("Please upload a valid image file (JPG, PNG, WEBP)."); return; }
    const reader = new FileReader();
    reader.onload = e => { setUploadedImage({ src:e.target.result, type:file.type }); setAiResult(null); setAiError(null); };
    reader.readAsDataURL(file);
  };

  const analyzeImage = async () => {
    if (!uploadedImage) return;
    setAnalyzing(true); setAiResult(null); setAiError(null);
    try {
      const base64 = uploadedImage.src.split(",")[1];
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method:"POST", headers:{ "Content-Type":"application/json" },
        body:JSON.stringify({
          model:"claude-sonnet-4-20250514", max_tokens:1000,
          messages:[{ role:"user", content:[
            { type:"image", source:{ type:"base64", media_type:uploadedImage.type, data:base64 } },
            { type:"text", text:`You are a soil science expert. Analyse this image carefully.
Respond ONLY with a raw JSON object (no markdown fences, no preamble):
{"isSoil":true,"soilType":"Sandy/Clay/Loamy/Silty/Peaty/Chalky/Unknown","confidence":0-100,"color":"description","texture":"description","quality":"Good/Moderate/Poor","qualityScore":0-100,"observations":["obs1","obs2","obs3"],"recommendations":["rec1","rec2"],"notSoilReason":"reason if not soil"}` }
          ]}]
        }),
      });
      const data = await res.json();
      if (data.error) throw new Error(data.error.message);
      const text = data.content.map(c => c.text||"").join("");
      setAiResult(JSON.parse(text.replace(/```json|```/g,"").trim()));
    } catch(e) {
      setAiError("Analysis failed: " + (e.message||"Please try again."));
    } finally { setAnalyzing(false); }
  };

  const qColor = s => s>=70?"#4CAF50":s>=40?"#FF9800":"#F44336";
  const qLabel = s => s>=70?"Good Quality ✅":s>=40?"Moderate Quality ⚠️":"Poor Quality ❌";

  return (
    <div style={{ fontFamily:"'Segoe UI',system-ui,sans-serif", background:"#f4f6f8", minHeight:"100vh" }}>
      {showPratham && <PrathamModal onClose={() => setShowPratham(false)} />}

      {/* NAV */}
      <div style={{ background:"linear-gradient(135deg,#1a3c34,#2E7D32,#1B5E20)", color:"white", padding:"0 1.5rem", position:"sticky", top:0, zIndex:50, boxShadow:"0 4px 20px rgba(0,0,0,0.3)" }}>
        <div style={{ maxWidth:1100, margin:"0 auto", display:"flex", alignItems:"center", justifyContent:"space-between", flexWrap:"wrap", gap:8, padding:"0.75rem 0" }}>
          <div>
            <div style={{ fontSize:20, fontWeight:900 }}>🌱 SoilSense</div>
            <div style={{ fontSize:10, opacity:0.7 }}>MIT Mysuru · Dept. Civil Engineering · Team GL Pratham</div>
          </div>
          <div style={{ display:"flex", gap:4, flexWrap:"wrap" }}>
            {nav.map(n => (
              <button key={n.id} onClick={() => setSection(n.id)} style={{
                background:section===n.id?"rgba(255,255,255,0.25)":"transparent",
                color:"white", border:section===n.id?"1px solid rgba(255,255,255,0.5)":"1px solid transparent",
                borderRadius:20, padding:"5px 12px", cursor:"pointer", fontSize:12, fontWeight:600,
              }}>{n.label}</button>
            ))}
          </div>
        </div>
      </div>

      <div style={{ maxWidth:1100, margin:"0 auto", padding:"1.5rem" }}>

        {/* HOME */}
        {section==="home" && (
          <div>
            <div style={{ background:"linear-gradient(135deg,#1B5E20,#388E3C,#66BB6A)", borderRadius:20, padding:"3rem 2rem", color:"white", textAlign:"center", marginBottom:"1.75rem" }}>
              <div style={{ fontSize:58, marginBottom:8 }}>🌍</div>
              <h1 style={{ margin:"0 0 0.5rem", fontSize:32, fontWeight:900 }}>Soil Science Explorer</h1>
              <p style={{ margin:"0 0 1.5rem", fontSize:16, opacity:0.9, maxWidth:540, marginLeft:"auto", marginRight:"auto" }}>
                AI-powered soil identification · 6 soil types · 6 testing methods · ML research insights
              </p>
              <div style={{ display:"flex", gap:12, justifyContent:"center", flexWrap:"wrap" }}>
                <button onClick={() => setSection("identify")} style={{ background:"white", color:"#2E7D32", border:"none", borderRadius:25, padding:"11px 26px", fontWeight:700, fontSize:14, cursor:"pointer" }}>🔬 Identify My Soil</button>
                <button onClick={() => setSection("soils")} style={{ background:"rgba(255,255,255,0.18)", color:"white", border:"2px solid white", borderRadius:25, padding:"11px 26px", fontWeight:700, fontSize:14, cursor:"pointer" }}>🌱 Explore Soil Types</button>
              </div>
            </div>

            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(175px,1fr))", gap:12, marginBottom:"1.75rem" }}>
              {[
                { label:"Soil Types", value:"6", icon:"🌍", bg:"#E8F5E9", accent:"#2E7D32" },
                { label:"Testing Methods", value:"6", icon:"🧪", bg:"#E3F2FD", accent:"#1565C0" },
                { label:"ML Algorithms", value:"5+", icon:"🤖", bg:"#F3E5F5", accent:"#6A1B9A" },
                { label:"World Records", value:"4", icon:"🏅", bg:"#FFF8E1", accent:"#E65100" },
              ].map(s => (
                <div key={s.label} style={{ background:s.bg, borderRadius:16, padding:"1.25rem", textAlign:"center", border:`2px solid ${s.accent}20` }}>
                  <div style={{ fontSize:30, marginBottom:4 }}>{s.icon}</div>
                  <div style={{ fontSize:26, fontWeight:900, color:s.accent }}>{s.value}</div>
                  <div style={{ fontSize:12, color:"#555", fontWeight:600 }}>{s.label}</div>
                </div>
              ))}
            </div>

            <div style={{ background:"white", borderRadius:20, padding:"1.5rem", boxShadow:"0 2px 10px rgba(0,0,0,0.06)", marginBottom:"1.5rem" }}>
              <h2 style={{ margin:"0 0 1rem", fontWeight:800, color:"#1B5E20", fontSize:19 }}>🌱 Quick Soil Snapshot</h2>
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(145px,1fr))", gap:10 }}>
                {soilTypes.map(soil => (
                  <div key={soil.id} onClick={() => { setSelectedSoil(soil); setSection("soils"); }}
                    style={{ background:soil.bg, border:`2px solid ${soil.border}`, borderRadius:14, padding:"0.9rem", cursor:"pointer", textAlign:"center", transition:"transform 0.2s" }}
                    onMouseEnter={e => e.currentTarget.style.transform="translateY(-4px)"}
                    onMouseLeave={e => e.currentTarget.style.transform=""}>
                    <div style={{ fontSize:28 }}>{soil.emoji}</div>
                    <div style={{ fontWeight:800, fontSize:12, color:soil.color, marginTop:4 }}>{soil.name}</div>
                    <div style={{ fontSize:10, color:"#999", marginTop:2 }}>Click to explore →</div>
                  </div>
                ))}
              </div>
            </div>

            <div style={{ background:"linear-gradient(135deg,#1a237e,#283593)", borderRadius:20, padding:"1.25rem 1.5rem", color:"white", display:"flex", alignItems:"center", justifyContent:"space-between", flexWrap:"wrap", gap:12 }}>
              <div>
                <div style={{ fontWeight:800, fontSize:16 }}>🚀 Publish This Website on GitHub — Free!</div>
                <div style={{ opacity:0.78, fontSize:13, marginTop:3 }}>Public URL, free forever, anyone can access — step-by-step guide inside</div>
              </div>
              <button onClick={() => setShowPublish(!showPublish)} style={{ background:"white", color:"#1a237e", border:"none", borderRadius:20, padding:"8px 20px", fontWeight:700, fontSize:13, cursor:"pointer" }}>
                {showPublish?"Hide Guide ▲":"Show Guide ▼"}
              </button>
            </div>
            {showPublish && <PublishGuide />}
          </div>
        )}

        {/* SOIL TYPES */}
        {section==="soils" && (
          <div>
            <h1 style={{ fontSize:26, fontWeight:900, color:"#1B5E20", margin:"0 0 1.25rem" }}>🌍 Types of Soil</h1>
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(285px,1fr))", gap:16, marginBottom:"1.5rem" }}>
              {soilTypes.map(soil => (
                <div key={soil.id} onClick={() => setSelectedSoil(selectedSoil?.id===soil.id?null:soil)}
                  style={{ background:"white", border:`3px solid ${selectedSoil?.id===soil.id?soil.color:"#e0e0e0"}`, borderRadius:18, overflow:"hidden", cursor:"pointer", boxShadow:selectedSoil?.id===soil.id?`0 8px 28px ${soil.color}30`:"0 2px 8px rgba(0,0,0,0.06)", transition:"all 0.25s" }}>
                  <div style={{ background:soil.color, height:7 }} />
                  <div style={{ padding:"1.2rem" }}>
                    <div style={{ display:"flex", gap:10, alignItems:"center", marginBottom:10 }}>
                      <div style={{ fontSize:36 }}>{soil.emoji}</div>
                      <div>
                        <div style={{ fontWeight:900, fontSize:16, color:soil.color }}>{soil.name}</div>
                        <div style={{ fontSize:12, color:"#777" }}>{soil.texture}</div>
                      </div>
                    </div>
                    {["sand","clay","silt"].map(k => (
                      <div key={k} style={{ marginBottom:5 }}>
                        <div style={{ display:"flex", justifyContent:"space-between", fontSize:11, marginBottom:2 }}>
                          <span style={{ color:"#666", textTransform:"capitalize" }}>{k}</span>
                          <span style={{ fontWeight:700 }}>{soil.characteristics[k]}%</span>
                        </div>
                        <div style={{ background:"#f0f0f0", borderRadius:6, height:7 }}>
                          <div style={{ background:k==="sand"?"#E6B800":k==="clay"?"#A0522D":"#7B8EC8", width:`${soil.characteristics[k]}%`, height:"100%", borderRadius:6 }} />
                        </div>
                      </div>
                    ))}
                    <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:6, marginTop:10 }}>
                      {[["Drainage",soil.drainage],["Fertility",soil.fertility],["pH Range",soil.pHRange],["Water Retention",soil.waterRetention]].map(([k,v]) => (
                        <div key={k} style={{ background:soil.bg, borderRadius:8, padding:"5px 8px", border:`1px solid ${soil.border}30` }}>
                          <div style={{ color:"#aaa", fontSize:9, fontWeight:700, textTransform:"uppercase" }}>{k}</div>
                          <div style={{ color:soil.color, fontWeight:800, fontSize:11 }}>{v}</div>
                        </div>
                      ))}
                    </div>
                  </div>
                </div>
              ))}
            </div>
            {selectedSoil && (
              <div style={{ background:"white", borderRadius:20, padding:"1.75rem", border:`3px solid ${selectedSoil.color}`, boxShadow:`0 8px 32px ${selectedSoil.color}20`, animation:"fadeIn .3s ease" }}>
                <div style={{ display:"flex", gap:"2rem", flexWrap:"wrap" }}>
                  <div style={{ flex:1, minWidth:240 }}>
                    <h2 style={{ fontSize:22, fontWeight:900, color:selectedSoil.color, margin:"0 0 0.5rem" }}>{selectedSoil.emoji} {selectedSoil.name}</h2>
                    <p style={{ color:"#555", lineHeight:1.7, marginBottom:"0.75rem" }}>{selectedSoil.description}</p>
                    <h3 style={{ fontSize:13, fontWeight:800, margin:"0 0 0.4rem" }}>✅ Best For</h3>
                    <div style={{ display:"flex", flexWrap:"wrap", gap:5, marginBottom:"0.75rem" }}>
                      {selectedSoil.bestFor.map(p => <span key={p} style={{ background:"#E8F5E9", color:"#2E7D32", borderRadius:20, padding:"3px 10px", fontSize:11, fontWeight:600 }}>{p}</span>)}
                    </div>
                    <h3 style={{ fontSize:13, fontWeight:800, margin:"0 0 0.4rem" }}>🔧 Improvements</h3>
                    <ul style={{ margin:0, paddingLeft:"1.25rem", color:"#555", lineHeight:1.8 }}>
                      {selectedSoil.improvements.map(i => <li key={i} style={{ fontSize:13 }}>{i}</li>)}
                    </ul>
                  </div>
                  <div style={{ width:240 }}>
                    <div style={{ background:selectedSoil.bg, borderRadius:14, padding:"1rem", border:`2px solid ${selectedSoil.border}35` }}>
                      <div style={{ fontWeight:800, fontSize:12, color:selectedSoil.color, marginBottom:8 }}>📊 Quick Stats</div>
                      {[["Organic Matter",selectedSoil.organic],["pH Range",selectedSoil.pHRange],["Water Retention",selectedSoil.waterRetention],["Drainage",selectedSoil.drainage]].map(([k,v]) => (
                        <div key={k} style={{ display:"flex", justifyContent:"space-between", fontSize:13, padding:"4px 0", borderBottom:"1px solid rgba(0,0,0,0.05)" }}>
                          <span style={{ color:"#777" }}>{k}</span>
                          <span style={{ fontWeight:700, color:selectedSoil.color }}>{v}</span>
                        </div>
                      ))}
                    </div>
                  </div>
                </div>
              </div>
            )}
          </div>
        )}

        {/* AI IDENTIFY */}
        {section==="identify" && (
          <div>
            <h1 style={{ fontSize:26, fontWeight:900, color:"#1B5E20", margin:"0 0 0.5rem" }}>🔬 AI Soil Identifier</h1>
            <p style={{ color:"#555", marginBottom:"1rem" }}>Upload a photo of soil — AI will identify type, quality score, and give actionable recommendations.</p>
            <div style={{ display:"flex", gap:8, marginBottom:"1.25rem", flexWrap:"wrap" }}>
              <span style={{ background:"#E8F5E9", border:"2px solid #4CAF50", borderRadius:25, padding:"5px 14px", fontSize:12, fontWeight:700, color:"#2E7D32" }}>✅ Level 1: Identification — Active</span>
              <span style={{ background:"#FFF8E1", border:"2px dashed #FF9800", borderRadius:25, padding:"5px 14px", fontSize:12, fontWeight:700, color:"#E65100" }}>🔜 Level 2: Full Report — Coming Soon</span>
            </div>
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:16 }}>
              <div>
                <div onDrop={e => { e.preventDefault(); handleFile(e.dataTransfer.files[0]); }} onDragOver={e => e.preventDefault()}
                  onClick={() => fileRef.current?.click()}
                  style={{ border:"3px dashed #4CAF50", borderRadius:18, padding:"2rem", textAlign:"center", cursor:"pointer", background:uploadedImage?"white":"#F1F8E9", minHeight:200, display:"flex", alignItems:"center", justifyContent:"center", flexDirection:"column", marginBottom:10 }}>
                  {uploadedImage
                    ? <img src={uploadedImage.src} alt="Soil" style={{ maxWidth:"100%", maxHeight:250, borderRadius:10, objectFit:"cover" }} />
                    : <><div style={{ fontSize:42, marginBottom:8 }}>📁</div><div style={{ fontWeight:700, color:"#2E7D32" }}>Drop soil image here</div><div style={{ color:"#888", fontSize:12, marginTop:4 }}>or click to browse · JPG PNG WEBP</div></>}
                </div>
                <input ref={fileRef} type="file" accept="image/*" onChange={e => handleFile(e.target.files[0])} style={{ display:"none" }} />
                {uploadedImage && (
                  <button onClick={analyzeImage} disabled={analyzing} style={{ width:"100%", background:analyzing?"#bbb":"linear-gradient(135deg,#2E7D32,#66BB6A)", color:"white", border:"none", borderRadius:14, padding:"12px", fontWeight:800, fontSize:15, cursor:analyzing?"not-allowed":"pointer" }}>
                    {analyzing?"🔄 Analysing…":"🤖 Analyse with AI"}
                  </button>
                )}
              </div>
              <div>
                {analyzing && <div style={{ background:"#E8F5E9", borderRadius:18, padding:"2rem", textAlign:"center" }}><div style={{ fontSize:42, marginBottom:10 }}>🔄</div><div style={{ fontWeight:700, color:"#2E7D32", fontSize:16 }}>AI is analysing your soil…</div><div style={{ color:"#666", marginTop:6, fontSize:13 }}>Examining texture, colour & composition</div></div>}
                {aiError && <div style={{ background:"#FFEBEE", borderRadius:18, padding:"1.25rem", border:"2px solid #F44336" }}><div style={{ fontWeight:700, color:"#B71C1C", marginBottom:6 }}>❌ Error</div><div style={{ color:"#555", fontSize:13 }}>{aiError}</div></div>}
                {aiResult && !analyzing && (
                  <div style={{ background:"white", borderRadius:18, border:"2px solid #4CAF50", overflow:"hidden" }}>
                    <div style={{ background:aiResult.isSoil?"linear-gradient(135deg,#2E7D32,#66BB6A)":"linear-gradient(135deg,#B71C1C,#EF5350)", padding:"1rem 1.25rem", color:"white" }}>
                      <div style={{ fontSize:19, fontWeight:900 }}>{aiResult.isSoil?`🌍 ${aiResult.soilType} Soil`:"⚠️ Not a soil image"}</div>
                      <div style={{ opacity:0.85, fontSize:13 }}>{aiResult.isSoil?`Confidence: ${aiResult.confidence}%`:aiResult.notSoilReason}</div>
                    </div>
                    {aiResult.isSoil && <div style={{ padding:"1.25rem" }}>
                      <div style={{ marginBottom:10 }}>
                        <div style={{ display:"flex", justifyContent:"space-between", fontSize:13, marginBottom:4 }}><span style={{ fontWeight:700 }}>Soil Quality</span><span style={{ fontWeight:900, color:qColor(aiResult.qualityScore) }}>{qLabel(aiResult.qualityScore)} ({aiResult.qualityScore}/100)</span></div>
                        <div style={{ background:"#f0f0f0", borderRadius:8, height:12 }}><div style={{ background:qColor(aiResult.qualityScore), width:`${aiResult.qualityScore}%`, height:"100%", borderRadius:8 }} /></div>
                      </div>
                      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:6, marginBottom:10 }}>
                        {[["Colour",aiResult.color],["Texture",aiResult.texture]].map(([k,v]) => (
                          <div key={k} style={{ background:"#F1F8E9", borderRadius:8, padding:"6px 10px" }}><div style={{ fontSize:10, color:"#888" }}>{k}</div><div style={{ fontSize:12, fontWeight:700, color:"#2E7D32" }}>{v}</div></div>
                        ))}
                      </div>
                      <div style={{ marginBottom:8 }}><div style={{ fontWeight:700, fontSize:13, marginBottom:4 }}>🔍 Observations</div>{aiResult.observations?.map((o,i) => <div key={i} style={{ fontSize:12, color:"#555", paddingLeft:14, position:"relative", marginBottom:2 }}><span style={{ position:"absolute", left:0 }}>•</span>{o}</div>)}</div>
                      <div><div style={{ fontWeight:700, fontSize:13, marginBottom:4 }}>💡 Recommendations</div>{aiResult.recommendations?.map((r,i) => <div key={i} style={{ fontSize:12, color:"#555", paddingLeft:14, position:"relative", marginBottom:2 }}><span style={{ position:"absolute", left:0, color:"#4CAF50" }}>✓</span>{r}</div>)}</div>
                    </div>}
                  </div>
                )}
                {!aiResult && !analyzing && !aiError && (
                  <div style={{ background:"#F3E5F5", borderRadius:18, padding:"2rem", textAlign:"center", border:"2px dashed #9C27B0" }}>
                    <div style={{ fontSize:42, marginBottom:10 }}>🤖</div>
                    <div style={{ fontWeight:700, color:"#6A1B9A", fontSize:15 }}>AI Analysis Ready</div>
                    <div style={{ color:"#666", fontSize:13, marginTop:6 }}>Upload a soil image to get instant identification, quality score, and recommendations.</div>
                  </div>
                )}
              </div>
            </div>
          </div>
        )}

        {/* TESTING */}
        {section==="testing" && (
          <div>
            <h1 style={{ fontSize:26, fontWeight:900, color:"#1B5E20", margin:"0 0 0.5rem" }}>🧪 Soil Testing Methods</h1>
            <p style={{ color:"#555", marginBottom:"1.25rem" }}>6 proven methods — from free DIY tests to professional lab analysis. Click any card for step-by-step instructions.</p>
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(285px,1fr))", gap:14 }}>
              {testingMethods.map((m,i) => (
                <div key={m.name} onClick={() => setExpandedTest(expandedTest===i?null:i)}
                  style={{ background:"white", borderRadius:18, overflow:"hidden", cursor:"pointer", border:`2px solid ${expandedTest===i?m.color:"#e0e0e0"}`, transition:"all 0.25s" }}>
                  <div style={{ background:m.color, padding:"0.85rem 1.25rem", display:"flex", alignItems:"center", gap:10 }}>
                    <span style={{ fontSize:28 }}>{m.icon}</span>
                    <div>
                      <div style={{ fontWeight:900, fontSize:15, color:"white" }}>{m.name}</div>
                      <div style={{ display:"flex", gap:6 }}>
                        <span style={{ background:"rgba(255,255,255,0.24)", borderRadius:12, padding:"1px 8px", fontSize:11, color:"white", fontWeight:700 }}>{m.difficulty}</span>
                        <span style={{ background:"rgba(255,255,255,0.24)", borderRadius:12, padding:"1px 8px", fontSize:11, color:"white", fontWeight:700 }}>{m.cost}</span>
                      </div>
                    </div>
                  </div>
                  <div style={{ padding:"1rem 1.25rem" }}>
                    <div style={{ fontSize:13, color:"#555", marginBottom:expandedTest===i?10:0 }}>📋 {m.result}</div>
                    {expandedTest===i && <div>
                      <div style={{ fontWeight:700, fontSize:12, color:m.color, marginBottom:8 }}>Step-by-Step:</div>
                      {m.steps.map((s,j) => (
                        <div key={j} style={{ display:"flex", gap:8, marginBottom:7 }}>
                          <div style={{ background:m.color, color:"white", borderRadius:"50%", width:20, height:20, display:"flex", alignItems:"center", justifyContent:"center", fontSize:10, fontWeight:900, flexShrink:0 }}>{j+1}</div>
                          <div style={{ fontSize:12, color:"#555", lineHeight:1.5 }}>{s}</div>
                        </div>
                      ))}
                    </div>}
                    <div style={{ marginTop:8, fontSize:11, color:m.color, fontWeight:700 }}>{expandedTest===i?"▲ Collapse":"▼ Show steps"}</div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* RESEARCH */}
        {section==="research" && (
          <div>
            <h1 style={{ fontSize:26, fontWeight:900, color:"#1B5E20", margin:"0 0 0.5rem" }}>📊 ML Research & Findings</h1>
            <p style={{ color:"#555", marginBottom:"1.25rem" }}>Based on peer-reviewed research papers on machine learning for soil classification.</p>
            <div style={{ background:"white", borderRadius:20, padding:"1.5rem", marginBottom:"1.25rem", boxShadow:"0 2px 10px rgba(0,0,0,0.06)" }}>
              <h2 style={{ fontSize:18, fontWeight:800, color:"#1a237e", margin:"0 0 1rem" }}>🤖 ML Algorithm Accuracy Comparison</h2>
              {mlAlgorithms.map(a => (
                <div key={a.name} style={{ marginBottom:"0.85rem" }}>
                  <div style={{ display:"flex", justifyContent:"space-between", fontSize:14, marginBottom:3 }}>
                    <span style={{ fontWeight:700 }}>{a.name}</span>
                    <span style={{ fontWeight:900, color:a.color }}>{a.accuracy}%</span>
                  </div>
                  <div style={{ background:"#f0f0f0", borderRadius:8, height:16, overflow:"hidden" }}>
                    <div style={{ background:a.color, width:`${a.accuracy}%`, height:"100%", borderRadius:8, display:"flex", alignItems:"center", justifyContent:"flex-end", paddingRight:8 }}>
                      <span style={{ fontSize:9, color:"white", fontWeight:800 }}>{a.accuracy}%</span>
                    </div>
                  </div>
                </div>
              ))}
            </div>
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(255px,1fr))", gap:14, marginBottom:"1.25rem" }}>
              {[
                { title:"🖼️ Multi-Feature Fusion", color:"#9C27B0", bg:"#F3E5F5", points:["Combining colour, texture & shape improves accuracy by ~15%","CNN extracts deep features automatically","Better than single-feature methods"] },
                { title:"📐 Texture Classification", color:"#1565C0", bg:"#E3F2FD", points:["GLCM captures soil texture effectively","Particle size distribution affects optical properties","Silty vs. Sandy achieves ~89% accuracy on texture alone"] },
                { title:"🎨 Colour Analysis", color:"#E65100", bg:"#FBE9E7", points:["Munsell colour charts used as ground truth","RGB + HSV colour spaces combined","Dark colour = high organic matter strongly"] },
              ].map(c => (
                <div key={c.title} style={{ background:c.bg, borderRadius:16, padding:"1.25rem", border:`2px solid ${c.color}20` }}>
                  <div style={{ fontWeight:900, fontSize:15, color:c.color, marginBottom:8 }}>{c.title}</div>
                  {c.points.map(p => <div key={p} style={{ fontSize:13, color:"#555", paddingLeft:14, position:"relative", lineHeight:1.6, marginBottom:3 }}><span style={{ position:"absolute", left:0, color:c.color }}>•</span>{p}</div>)}
                </div>
              ))}
            </div>
            <div style={{ background:"#1a237e", borderRadius:18, padding:"1.5rem", color:"white" }}>
              <h2 style={{ fontSize:16, fontWeight:800, margin:"0 0 0.75rem" }}>📚 Research Papers Referenced</h2>
              {["Comparison of ML Algorithms for Soil Type Classification","Multi-Feature Fusion for Soil Image Feature Extraction and Classification Using ML","Classification of Soil Texture Using Machine Learning Technique","Deep Learning Approaches for Geotechnical Soil Analysis","Soil Physical Properties and ML-Based Quality Prediction"].map((p,i) => (
                <div key={p} style={{ display:"flex", gap:10, marginBottom:7, alignItems:"flex-start" }}>
                  <span style={{ background:"rgba(255,255,255,0.15)", borderRadius:"50%", width:22, height:22, display:"flex", alignItems:"center", justifyContent:"center", fontSize:10, fontWeight:900, flexShrink:0 }}>{i+1}</span>
                  <span style={{ fontSize:13, opacity:0.85, lineHeight:1.6 }}>{p}</span>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* TEAM */}
        {section==="team" && (
          <div>
            <h1 style={{ fontSize:26, fontWeight:900, color:"#1B5E20", margin:"0 0 0.25rem" }}>👥 Our Team</h1>
            <p style={{ color:"#555", marginBottom:"1.25rem" }}>
              B.E. Civil Engineering · Maharaja Institute of Technology Mysuru (MITM) · Karnataka<br />
              <strong>Under the Guidance of Prof. Murul Gyes (Varun Sir)</strong>, Associate Professor, Dept. of Civil Engineering, MIT Mysuru
            </p>

            {/* Mentor */}
            <div style={{ background:"linear-gradient(135deg,#1a237e,#283593)", borderRadius:18, padding:"1.25rem 1.5rem", color:"white", marginBottom:"1.5rem", display:"flex", gap:14, alignItems:"center", flexWrap:"wrap" }}>
              <div style={{ fontSize:46 }}>👨‍🏫</div>
              <div>
                <div style={{ fontWeight:900, fontSize:18 }}>Prof. Murul Gyes (Varun Sir)</div>
                <div style={{ opacity:0.85, fontSize:13, marginTop:2 }}>Associate Professor · Department of Civil Engineering</div>
                <div style={{ opacity:0.8, fontSize:13 }}>Maharaja Institute of Technology Mysuru (MITM) · Mysore, Karnataka</div>
                <span style={{ display:"inline-block", marginTop:7, background:"rgba(255,255,255,0.17)", borderRadius:20, padding:"3px 12px", fontSize:11, fontWeight:700 }}>Project Guide & Mentor</span>
              </div>
            </div>

            {/* Members grid */}
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(210px,1fr))", gap:12, marginBottom:"1.5rem" }}>
              {teamMembers.map(m => (
                <div key={m.name}
                  onClick={m.isLeader?() => setShowPratham(true):undefined}
                  style={{ background:m.bg, borderRadius:18, padding:"1.4rem", textAlign:"center", border:`2px solid ${m.border}45`, cursor:m.isLeader?"pointer":"default", transition:"transform .2s,box-shadow .2s", position:"relative" }}
                  onMouseEnter={e => { if(m.isLeader){ e.currentTarget.style.transform="translateY(-4px)"; e.currentTarget.style.boxShadow=`0 8px 24px ${m.color}35`; } }}
                  onMouseLeave={e => { e.currentTarget.style.transform=""; e.currentTarget.style.boxShadow=""; }}>
                  {m.isLeader && <div style={{ position:"absolute", top:9, right:9, background:m.color, color:"white", borderRadius:20, padding:"2px 8px", fontSize:9, fontWeight:800 }}>GL · LEADER</div>}
                  <div style={{ fontSize:42, marginBottom:7 }}>{m.emoji}</div>
                  <div style={{ fontWeight:900, fontSize:15, color:m.color }}>{m.name}</div>
                  <div style={{ fontSize:12, color:"#666", marginTop:3 }}>{m.role}</div>
                  <div style={{ fontSize:10, color:"#aaa", marginTop:2, fontFamily:"monospace" }}>{m.usn}</div>
                  {m.isLeader && <div style={{ marginTop:8 }}>
                    <div style={{ fontSize:10, color:"#E65100", fontWeight:700 }}>🏅 World Record Holder · 🎥 YouTuber</div>
                    <div style={{ fontSize:10, color:m.color, marginTop:3, fontWeight:600 }}>Click to view full profile →</div>
                  </div>}
                </div>
              ))}
            </div>

            {/* GL Pratham about section */}
            <div style={{ background:"white", borderRadius:20, padding:"1.5rem", border:"3px solid #FFB300", boxShadow:"0 4px 20px rgba(255,179,0,0.12)" }}>
              <div style={{ display:"flex", gap:14, alignItems:"center", marginBottom:"1rem", flexWrap:"wrap" }}>
                <div style={{ fontSize:42 }}>🏆</div>
                <div style={{ flex:1 }}>
                  <div style={{ fontWeight:900, fontSize:19, color:"#E65100" }}>GL Pratham — Team Leader</div>
                  <div style={{ fontSize:12, color:"#888" }}>USN: 4MH23CV016 · B.E. Civil Engineering · MIT Mysuru</div>
                </div>
                <button onClick={() => setShowPratham(true)} style={{ background:"#E65100", color:"white", border:"none", borderRadius:20, padding:"8px 18px", fontWeight:700, fontSize:13, cursor:"pointer" }}>Full Profile →</button>
              </div>
              <p style={{ color:"#555", lineHeight:1.75, fontSize:14, marginBottom:"1rem" }}>
                GL Pratham is a Civil Engineering student at <strong>Maharaja Institute of Technology Mysuru (MITM)</strong>, Mysore, Karnataka.
                He is the Group Leader for this soil science research project, combining geotechnical engineering with AI and machine learning.
                A passionate content creator from Karnataka, he runs the YouTube channel <strong>Prathamgowda001</strong> producing vlogs,
                informative content, engineering topics, and science videos. He is also a multi-record holder, having set world records at the national level in 2025.
              </p>
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(195px,1fr))", gap:10 }}>
                <div style={{ background:"#FFF3E0", borderRadius:14, padding:"0.9rem", border:"2px solid #FFB300", display:"flex", gap:10, alignItems:"center" }}>
                  <div style={{ fontSize:26 }}>▶️</div>
                  <div>
                    <div style={{ fontWeight:800, color:"#BF360C", fontSize:12 }}>Prathamgowda0001</div>
                    <div style={{ fontSize:11, color:"#777" }}>YouTube · Karnataka, India</div>
                    <div style={{ fontSize:11, color:"#888" }}>Vlogs · Science · Engineering</div>
                  </div>
                </div>
                <div style={{ background:"#FFF8E1", borderRadius:14, padding:"0.9rem", border:"2px solid #B8860B", display:"flex", gap:10, alignItems:"center" }}>
                  <div style={{ fontSize:26 }}>🏅</div>
                  <div>
                    <div style={{ fontWeight:800, color:"#B8860B", fontSize:12 }}>4× World Record Certificates</div>
                    <div style={{ fontSize:11, color:"#777" }}>INWR · Kings World Records</div>
                    <div style={{ fontSize:11, color:"#888" }}>Record No: IN24-103-423</div>
                  </div>
                </div>
                <div style={{ background:"#E8F5E9", borderRadius:14, padding:"0.9rem", border:"2px solid #4CAF50", display:"flex", gap:10, alignItems:"center" }}>
                  <div style={{ fontSize:26 }}>🎨</div>
                  <div>
                    <div style={{ fontWeight:800, color:"#2E7D32", fontSize:12 }}>World's Largest Jio Portrait</div>
                    <div style={{ fontSize:11, color:"#777" }}>Sir M. Visvesvaraya</div>
                    <div style={{ fontSize:11, color:"#888" }}>MITM Sports Ground, Mysuru</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        )}

      </div>

      {/* FOOTER */}
      <div style={{ background:"#111", color:"#999", textAlign:"center", padding:"1.5rem", marginTop:"2rem", fontSize:12 }}>
        <div style={{ fontWeight:700, color:"white", marginBottom:4 }}>🌱 SoilSense — Soil Science Explorer</div>
        <div>B.E. Civil Engineering · Maharaja Institute of Technology Mysuru (MITM) · Mysore, Karnataka</div>
        <div style={{ marginTop:3 }}>Team Leader: GL Pratham (4MH23CV016) · Bharathi S (4MH23CV009) · Poorvika (4MH24CV408) · Vinod S (4MH24CV417)</div>
        <div style={{ marginTop:3 }}>Guided by Prof.Varun Sir — Associate Professor, Dept. of Civil Engineering, MIT Mysuru</div>
        <div style={{ marginTop:6, color:"#555", fontSize:11 }}>Built with AI · Educational Project · Karnataka, India</div>
      </div>

      <style>{`@keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}`}</style>
    </div>
  );
}
