
const html = fs.readFileSync('outputs/دولة-من-الصفر.html', 'utf8');
const source = html.match(/<script>([\s\S]*?)<\/script>/)[1].replace('window.advanceMonth=advanceMonth;', 'window.__test={getState:()=>s,setTreasury:v=>s.treasury=v,setRelation:(id,v)=>{s.countries.find(c=>c.id===id).relation=v},selectCountry:id=>selectedCountry=id,resetProjects:()=>{s.projects=[]},render:()=>renderAll(),getProjects:()=>projectCatalog,makeState:f=>initialState(f)};window.advanceMonth=advanceMonth;');
const checks = ['dir="rtl"','@media(max-width:760px)','data-project','window.advanceMonth','function diplomaticAction','function generateEvent','localStorage.setItem','function renderSecurity','function renderNews'];
const source = html.match(/<script>([\s\S]*?)<\/script>/)[1].replace('window.advanceMonth=advanceMonth;', 'window.__test={getState:()=>s,setState:v=>s=v,setTreasury:v=>s.treasury=v,setRelation:(id,v)=>{s.countries.find(c=>c.id===id).relation=v},setOffer:(kind,id)=>{s.pendingOffer={countryId:id||"nour",kind,issuedTurn:s.turn,title:"Test offer",body:"Test terms"}},selectCountry:id=>selectedCountry=id,resetProjects:()=>{s.projects=[]},render:()=>renderAll(),getProjects:()=>projectCatalog,makeState:f=>initialState(f),getBudget:()=>({income:calcIncome(),expenses:calcExpenses()}),checkMandate,getMandateStage:()=>s.mandateStage,getMandateTracks:()=>mandateTracks};window.advanceMonth=advanceMonth;');
const checks = ['dir="rtl"','@media(max-width:760px)','class="map-land"','مستشار مجلس الوزراء','خطة التأسيس الوطنية','function checkMandate','data-project','window.advanceMonth','function diplomaticAction','function generateEvent','localStorage.setItem','function renderSecurity','function renderNews'];
for (const c of checks) assert(html.includes(c), `Missing feature: ${c}`);
  set oninput(fn){this.handlers.input=fn} get oninput(){return this.handlers.input}
  scrollIntoView(){}
  scrollIntoView(){this.scrolled=true}
  remove(){this.removed=true}
const projects=['school','hospital','solar','port','farm','water','road','factory','barracks','digital'].map(k=>new El('',{project:k}));
const countries=['nour','sahil','qamar','jabal','waha','sahra','sharq'].flatMap(k=>[new El('',{country:k}),new El('',{selectCountry:k})]);
const countries=['usa','canada','mexico','brazil','uk','france','germany','italy','turkiye','russia','china','india','japan','indonesia','saudi','uae','egypt','southafrica','nigeria','australia'].flatMap(k=>[new El('',{country:k}),new El('',{selectCountry:k})]);
const document={
};
byId('countryName').value='مملكة الاختبار';byId('leaderName').value='قائد تجريبي';byId('region').value='coast';byId('government').value='republic';byId('priority').value='balanced';byId('difficulty').value='normal';
byId('countryName').value='مملكة الاختبار';byId('leaderName').value='قائد تجريبي';byId('capital').value='دار السلام';byId('flag').value='🌟';byId('region').value='coast';byId('government').value='republic';byId('priority').value='balanced';byId('difficulty').value='normal';
const storage=new Map();
assert(byId('topDate').textContent==='يناير 2026','dashboard renders the starting date');
assert(context.__test.getState().countries.length===20,'the world contains twenty real countries');
assert(context.__test.getState().capital==='دار السلام'&&context.__test.getState().flag==='🌟','custom capital and national symbol are saved');
assert(byId('view-world').innerHTML.includes('الولايات المتحدة')&&byId('view-world').innerHTML.includes('التكتلات والتحالفات الدولية'),'the rendered world shows real countries and alliances');
assert(byId('view-overview').innerHTML.includes('دليل البداية')&&byId('view-overview').innerHTML.includes('مستشار مجلس الوزراء'),'guided start and contextual help are visible');
assert(byId('view-overview').innerHTML.includes('المرحلة 1: تأمين أساسيات الدولة'),'the starting mandate explains the first national objective');
countries.find(x=>x.dataset.selectCountry==='usa').onclick();assert(context.__test.getState().countries.length===20&&byId('diplomacyCard').scrolled,'selecting a world country opens its dossier and scrolls it into view');
context.toggleGuide();assert(byId('view-overview').innerHTML.includes('دليل البداية مخفي'),'start guide can be hidden');context.toggleGuide();
const beginner=context.__test.makeState({country:'تجربة',leader:'قائد',region:'coast',government:'republic',priority:'balanced',difficulty:'beginner'});
assert(beginner.treasury===3600&&context.__test.getBudget().income>=context.__test.getBudget().expenses,'beginner mode starts with a generous reserve and sustainable monthly budget');
const baseForm={country:'تجربة',leader:'قائد',region:'coast',government:'republic',priority:'balanced',difficulty:'normal'};
for(const [region,x,y,key] of [['coast',42,49,'trade'],['plain',37,72,'food'],['mountain',65,57,'energy'],['desert',61,56,'energy']]){const place=context.__test.makeState({...baseForm,region});assert(place.mapX===x&&place.mapY===y&&place.region===region,`${region} starts at its world-map coordinates`);assert(typeof place[key]==='number',`${region} supplies its advertised resource`)}
const republic=context.__test.makeState(baseForm),federal=context.__test.makeState({...baseForm,government:'federal'}),monarchy=context.__test.makeState({...baseForm,government:'monarchy'}),council=context.__test.makeState({...baseForm,government:'council'});
assert(federal.governance>republic.governance&&monarchy.legitimacy>republic.legitimacy&&council.legitimacy<republic.legitimacy,'government systems produce distinct starting tradeoffs');
const originalGameState=context.__test.getState();
for(const priority of ['balanced','industry','welfare','defense']){const mandateState=context.__test.makeState({...baseForm,priority,difficulty:'beginner'});context.__test.setState(mandateState);const track=context.__test.getMandateTracks()[priority];for(let i=0;i<track.length;i++){const m=track[i];for(const goal of m.goals){if(goal[0]==='allianceCount')mandateState.alliances=Array.from({length:goal[2]},(_,n)=>'test-ally-'+n);else if(goal[0]==='treatyCount')mandateState.treaties=Array.from({length:goal[2]},(_,n)=>'test-treaty-'+n);else mandateState[goal[0]]=goal[2]}const treasuryBefore=mandateState.treasury,approvalBefore=mandateState.approval;assert(context.__test.checkMandate(),`${priority} mandate stage ${i+1} completes when its conditions are met`);assert(mandateState.mandateStage===i+1&&mandateState.treasury===treasuryBefore+m.reward&&mandateState.approval===approvalBefore+2,`${priority} stage ${i+1} pays its reward once`)}context.__test.render();assert(!context.__test.checkMandate()&&byId('view-overview').innerHTML.includes('اكتملت ولايتك التأسيسية'),`${priority} campaign ends once and shows its completed state`)}
context.__test.setState(originalGameState);context.__test.render();
for(const tab of tabs) context.setTab(tab);

assert(context.__test.getState().pendingOffer,'an international offer arrives as the world simulates');
context.__test.render();assert(byId('view-overview').innerHTML.includes('عرض دولي'),'incoming offer appears on the dashboard');
context.resolveOffer('accept');byId('modalYes').onclick();
assert(!context.__test.getState().pendingOffer,'incoming offer can be accepted');
for(const [kind,id] of [['loan','canada'],['grant','brazil'],['alliance','saudi']]){
  context.__test.setOffer(kind,id);context.__test.render();
  const before=JSON.parse(JSON.stringify(context.__test.getState()));
  context.resolveOffer('accept');byId('modalYes').onclick();
  const after=context.__test.getState();assert(!after.pendingOffer,`${kind} offer closes after acceptance`);
  if(kind==='loan')assert(after.debt===before.debt+290,'loan offer records debt terms');
  if(kind==='grant')assert(after.education===Math.min(100,before.education+5),'grant adds education without debt');
  if(kind==='alliance')assert(after.alliances.includes(id),'defense offer forms a formal alliance');
}
context.__test.setOffer('trade','canada');const relationBeforeDecline=context.__test.getState().countries.find(c=>c.id==='canada').relation;context.resolveOffer('decline');assert(!context.__test.getState().pendingOffer&&context.__test.getState().countries.find(c=>c.id==='canada').relation===relationBeforeDecline-1&&context.__test.getState().news.some(n=>n.title==='رفض عرض كندا'),'international offers can be declined politely with a small diplomatic effect');

// Exercise every diplomatic action and strategic action.
  context.__test.setTreasury(5000);context.__test.render();
  context.__test.setRelation('nour',10);
  const relBefore=context.__test.getState().countries.find(c=>c.id==='nour').relation;
  context.diplomaticAction('nour',action);byId('modalYes').onclick();
  if(action==='trade'){assert(context.__test.getState().treaties.includes('nour'),'trade treaty recorded');assert(!context.__test.getState().alliances?.includes('nour'),'trade does not imply a military alliance');context.__test.selectCountry('nour');context.__test.render();assert(byId('view-world').innerHTML.includes('اقتراح تحالف'),'trade does not label the country an ally');}
  if(action==='aid')assert(context.__test.getState().countries.find(c=>c.id==='nour').relation>relBefore,'aid improves relations');
  if(action==='alliance'){assert(context.__test.getState().alliances.includes('nour'),'alliance recorded');context.__test.selectCountry('nour');context.__test.render();assert(byId('view-world').innerHTML.includes('حليف قائم'),'formal alliance is displayed');}
  if(action==='sanction')assert(context.__test.getState().countries.find(c=>c.id==='nour').relation<relBefore,'sanctions reduce relations');
  context.__test.setRelation('usa',10);
  const relBefore=context.__test.getState().countries.find(c=>c.id==='usa').relation;
  context.diplomaticAction('usa',action);byId('modalYes').onclick();
  if(action==='trade'){assert(context.__test.getState().treaties.includes('usa'),'trade treaty recorded');assert(!context.__test.getState().alliances?.includes('usa'),'trade does not imply a military alliance');context.__test.selectCountry('usa');context.__test.render();assert(byId('view-world').innerHTML.includes('اقتراح تحالف'),'trade does not label the country an ally');}
  if(action==='aid')assert(context.__test.getState().countries.find(c=>c.id==='usa').relation>relBefore,'aid improves relations');
  if(action==='alliance'){assert(context.__test.getState().alliances.includes('usa'),'alliance recorded');context.__test.selectCountry('usa');context.__test.render();assert(byId('view-world').innerHTML.includes('حليف قائم'),'formal alliance is displayed');}
  if(action==='sanction')assert(context.__test.getState().countries.find(c=>c.id==='usa').relation<relBefore,'sanctions reduce relations');
}
assert(context.__test.getState().corruption<33,'anti-corruption initiative works');
context.__test.setTreasury(20);
for(let i=0;i<3;i++)context.advanceMonth();
assert(context.__test.getState().pendingEvent,'a national event arrives on schedule');
context.__test.setTreasury(20);
const lowTreasury=context.__test.getState().treasury;context.resolveEvent(0);
assert(storage.has('nationBuilderSaveV1'),'progress autosaves');
const savedTurn=context.__test.getState().turn;
const savedTurn=context.__test.getState().turn,savedMandateStage=context.__test.getState().mandateStage;
const reboot={document,localStorage:context.localStorage,console,Math:math,Intl,Number,JSON,setTimeout:()=>0,clearTimeout:()=>{},window:null};reboot.window=reboot;vm.createContext(reboot);vm.runInContext(source,reboot);
assert(reboot.__test.getState().turn===savedTurn,'saved game reloads at the same turn');
assert(reboot.__test.getState().turn===savedTurn&&reboot.__test.getState().mandateStage===savedMandateStage,'saved game reloads at the same turn and campaign stage');
const saveForMigration=JSON.parse(JSON.stringify(reboot.__test.getState())),legacy=JSON.parse(JSON.stringify(saveForMigration));legacy.stateVersion=1;legacy.countries=legacy.countries.slice(0,7);legacy.treasury=500;delete legacy.capital;delete legacy.flags;delete legacy.pendingOffer;delete legacy.stateVersion;
byId('newBtn').onclick();byId('modalYes').onclick();
assert(!storage.has('nationBuilderSaveV1'),'new game replaces the saved state after confirmation');
console.log(`PASS: ${checks.length} static checks; all views; budget and policy effects; all ${projects.length} projects; all diplomatic actions; event affordability and resolution; autonomous world news; month progression; save, reload, and reset.`);
const legacyData=new Map([['nationBuilderSaveV1',JSON.stringify(legacy)]]),legacyStorage={setItem:(k,v)=>legacyData.set(k,v),getItem:k=>legacyData.get(k)||null,removeItem:k=>legacyData.delete(k)};
const upgraded={document,localStorage:legacyStorage,console,Math:math,Intl,Number,JSON,setTimeout:()=>0,clearTimeout:()=>{},window:null};upgraded.window=upgraded;vm.createContext(upgraded);vm.runInContext(source,upgraded);
assert(upgraded.__test.getState().countries.length===20&&upgraded.__test.getState().capital==='نوران'&&upgraded.__test.getState().treasury>=2300,'older saves migrate to the expanded world and guided start');
const saveV3=JSON.parse(JSON.stringify(saveForMigration));saveV3.stateVersion=3;delete saveV3.mandateStage;const v3Data=new Map([['nationBuilderSaveV1',JSON.stringify(saveV3)]]),v3Storage={setItem:(k,v)=>v3Data.set(k,v),getItem:k=>v3Data.get(k)||null,removeItem:k=>v3Data.delete(k)};const upgradedV3={document,localStorage:v3Storage,console,Math:math,Intl,Number,JSON,setTimeout:()=>0,clearTimeout:()=>{},window:null};upgradedV3.window=upgradedV3;vm.createContext(upgradedV3);vm.runInContext(source,upgradedV3);assert(upgradedV3.__test.getState().stateVersion===4&&upgradedV3.__test.getState().mandateStage===0&&upgradedV3.__test.getState().priority===saveV3.priority,'version-three saves migrate safely to the national campaign system');
console.log(`PASS: ${checks.length} static checks; beginner economy; 20-country real-world map; guided start and help; clickable country dossiers; four regional starts; all four three-stage national mandate campaigns and rewards; identity; all views; budget and policy effects; all ${projects.length} projects; diplomacy and all four offer types including refusal; event affordability; autonomous world news; save, reload, reset, and upgrade.`);
