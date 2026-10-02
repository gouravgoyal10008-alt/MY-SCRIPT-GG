local R=loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local W=R:CreateWindow({Name="MY MENU GG",LoadingTitle="MY MENU GG",LoadingSubtitle="by Community",ConfigurationSaving={Enabled=false},KeySystem=false})
local Tm,Tv,Tu,Ta,Ts=W:CreateTab("Movement Hacks",4483362458),W:CreateTab("Visuals & ESP",4483362458),W:CreateTab("Scripts & Tools",4483362458),W:CreateTab("Anti & Protection",4483362458),W:CreateTab("Server Hop",4483362458)
local Plrs,RunS,UIS,Wksp,Lght,Tele,Http,VUsr=game:GetService("Players"),game:GetService("RunService"),game:GetService("UserInputService"),game:GetService("Workspace"),game:GetService("Lighting"),game:GetService("TeleportService"),game:GetService("HttpService"),game:GetService("VirtualUser")
local LP,Cam=Plrs.LocalPlayer,Wksp.CurrentCamera
local sEn,sVal,iJmp,jVal,tSpam,tStuds,wHop,wAct=false,16,false,50,false,1,false,false
local pltEn,pltCol,nclp=false,Color3.fromRGB(255,255,255),false
local playerEsp,bEsp,bCol=false,false,Color3.new(0,0,0)
local fbEn,nfEn,xRayEn=false,false,false
local aAfk,aSlw,aRag=true,true,false
local hbEn,hbSize=false,2
local origSizes,hbOutlines,espContainers={},{},{}
local cMap={White=Color3.new(1,1,1),Red=Color3.new(1,0,0),Blue=Color3.new(0,0,1),Green=Color3.new(0,1,0),Yellow=Color3.new(1,1,0),Purple=Color3.fromRGB(128,0,128),Orange=Color3.fromRGB(255,165,0),Pink=Color3.fromRGB(255,192,203),Cyan=Color3.fromRGB(0,255,255),Black=Color3.new(0,0,0)}
local cList={"White","Red","Blue","Green","Yellow","Purple","Orange","Pink","Cyan","Black"}

local function getTgt()
local mLoc=UIS:GetMouseLocation()
local uRay=Cam:ViewportPointToRay(mLoc.X,mLoc.Y)
local rPar=RaycastParams.new() rPar.FilterType=Enum.RaycastFilterType.Exclude
local ign={LP.Character}
for _,o in ipairs(Wksp:GetDescendants()) do if o:IsA("BasePart") and (not o.CanCollide or o.Transparency>0.5) then table.insert(ign,o) end end
rPar.FilterDescendantsInstances=ign
local res=Wksp:Raycast(uRay.Origin,uRay.Direction*1000,rPar)
return res and res.Instance,res and res.Position
end

Tm:CreateToggle({Name="Speed Toggle",CurrentValue=false,Callback=function(v) sEn=v while sEn and task.wait(0.1) do local c=LP.Character if c and c:FindFirstChild("Humanoid") then c.Humanoid.WalkSpeed=sVal end end if not sEn and LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.WalkSpeed=16 end end})
Tm:CreateSlider({Name="Speed Level",Range={16,100},Increment=1,Suffix="Speed",CurrentValue=16,Callback=function(v) sVal=v end})
Tm:CreateToggle({Name="Infinite Jump",CurrentValue=false,Callback=function(v) iJmp=v end})
UIS.JumpRequest:Connect(function()
if (iJmp or wAct) and LP.Character and LP.Character:FindFirstChild("Humanoid") then
local h=LP.Character.Humanoid h.UseJumpPower=true h.JumpPower=jVal h:ChangeState(Enum.HumanoidStateType.Jumping)
end
end)
Tm:CreateSlider({Name="High Jump Level",Range={50,100},Increment=1,Suffix="Power",CurrentValue=50,Callback=function(v) jVal=v end})
Tm:CreateToggle({Name="Teleport in Front",CurrentValue=false,Callback=function(v) tSpam=v while tSpam and task.wait(0.05) do local c=LP.Character if c and c:FindFirstChild("HumanoidRootPart") and c:FindFirstChild("Humanoid") and c.Humanoid.MoveDirection.Magnitude>0 then c.HumanoidRootPart.CFrame=c.HumanoidRootPart.CFrame+(c.HumanoidRootPart.CFrame.LookVector*tStuds) end end end})
Tm:CreateSlider({Name="Teleport Distance (Studs)",Range={1,100},Increment=1,Suffix="Studs",CurrentValue=1,Callback=function(v) tStuds=v end})
Tm:CreateToggle({Name="Auto Wall Hop (2-Stud Inf Jump)",CurrentValue=false,Callback=function(v) wHop=v if not v then wAct=false end end})

RunS.RenderStepped:Connect(function()
if not wHop then wAct=false return end
local c=LP.Character if not c or not c:FindFirstChild("HumanoidRootPart") then wAct=false return end
local hrp=c.HumanoidRootPart local p=RaycastParams.new() p.FilterType=Enum.RaycastFilterType.Exclude p.FilterDescendantsInstances={c}
local l,r,d=hrp.CFrame.LookVector,hrp.CFrame.RightVector,2.0
local near=false
for _,dir in ipairs({l*d,-l*d,r*d,-r*d,(l+r).Unit*d,(l-r).Unit*d,(-l+r).Unit*d,(-l-r).Unit*d}) do
local res=Wksp:Raycast(hrp.Position,dir,p)
if res and res.Instance and res.Instance.Anchored and res.Instance.CanCollide then near=true break end
end
wAct=near
end)

Tm:CreateDropdown({Name="Platform Color",Options=cList,CurrentOption="White",Callback=function(o) local s=type(o)=="table" and o[1] or o if cMap[s] then pltCol=cMap[s] end end})
Tm:CreateToggle({Name="Platform Spawner",CurrentValue=false,Callback=function(v) pltEn=v while pltEn and task.wait(0.1) do local c=LP.Character if c and c:FindFirstChild("HumanoidRootPart") then local pt=Instance.new("Part") pt.Size=Vector3.new(3,0.5,3) pt.Position=c.HumanoidRootPart.Position-Vector3.new(0,3.2,0) pt.Anchored=true pt.Color=pltCol pt.Transparency=0.2 pt.Parent=Wksp task.delay(0.4,function() pt:Destroy() end) end end end})
Tm:CreateToggle({Name="Noclip",CurrentValue=false,Callback=function(v) nclp=v end})
RunS.Stepped:Connect(function() if nclp and LP.Character then for _,p in ipairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide=false end end end end)

Plrs.PlayerRemoving:Connect(function(lp) if espContainers[lp] then espContainers[lp]:Destroy() espContainers[lp]=nil end end)

Tv:CreateToggle({Name="Player ESP",CurrentValue=false,Callback=function(v) playerEsp=v if not v then for _,ct in pairs(espContainers) do if ct then ct:Destroy() end end espContainers={} end end})
Tv:CreateToggle({Name="Hitbox Expander",CurrentValue=false,Callback=function(v) hbEn=v if not hbEn then for _,p in ipairs(Plrs:GetPlayers()) do if p.Character then local hrp=p.Character:FindFirstChild("HumanoidRootPart") if hrp and origSizes[hrp] then hrp.Size=origSizes[hrp] hrp.Transparency=0 hrp.CanCollide=true end end end origSizes={} end end})
Tv:CreateSlider({Name="Hitbox Size",Range={2,200},Increment=1,Suffix="Studs",CurrentValue=2,Callback=function(v) hbSize=v end})

task.spawn(function()
while true do
if hbEn then
for _,p in ipairs(Plrs:GetPlayers()) do
if p~=LP and p.Character then
local hrp=p.Character:FindFirstChild("HumanoidRootPart")
if hrp then
if not origSizes[hrp] then origSizes[hrp]=hrp.Size end
hrp.Size=Vector3.new(hbSize,hbSize,hbSize) hrp.Transparency=1 hrp.CanCollide=false
end
end
end
end
task.wait(0.2)
end
end)

Tv:CreateDropdown({Name="Block ESP Color",Options=cList,CurrentOption="Black",Callback=function(o) local s=type(o)=="table" and o[1] or o if cMap[s] then bCol=cMap[s] for _,b in pairs(bBxs) do if b and b.Parent then b.Color3=bCol end end end end})
local bBxs={}
Tv:CreateToggle({Name="Block ESP (Solid Only)",CurrentValue=false,Callback=function(v)
bEsp=v
if v then task.spawn(function() for _,obj in ipairs(Wksp:GetDescendants()) do if not bEsp then break end if obj:IsA("BasePart") and obj.CanCollide and obj.Anchored and not obj:IsDescendantOf(LP.Character or Wksp) then local isP=false for _,p in ipairs(Plrs:GetPlayers()) do if p.Character and obj:IsDescendantOf(p.Character) then isP=true break end end if not isP and not bBxs[obj] then local bx=Instance.new("SelectionBox") bx.Name="BlockOutlineESP" bx.Adornee=obj bx.Color3=bCol bx.LineThickness=0.05 bx.SurfaceTransparency=1 bx.Parent=obj bBxs[obj]=bx end end end end)
else for _,b in pairs(bBxs) do if b then b:Destroy() end end bBxs={} end
end})

local origTrans={}
Tv:CreateToggle({Name="Simple X-Ray",CurrentValue=false,Callback=function(v)
xRayEn=v
if xRayEn then origTrans={} for _,obj in ipairs(Wksp:GetDescendants()) do if obj:IsA("BasePart") and not obj:IsDescendantOf(LP.Character or Wksp) then local isCP=false for _,p in ipairs(Plrs:GetPlayers()) do if p.Character and obj:IsDescendantOf(p.Character) then isCP=true break end end if not isCP then origTrans[obj]=obj.Transparency obj.Transparency=0.75 end end end
else for obj,trans in pairs(origTrans) do if obj and obj.Parent then obj.Transparency=trans end end origTrans={} end
end})

RunS.RenderStepped:Connect(function()
local boxColor=Color3.fromRGB(255,0,0)
if hbEn then
for _,p in ipairs(Plrs:GetPlayers()) do
if p~=LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
local hrp=p.Character.HumanoidRootPart local outline=hbOutlines[p]
if not outline or not outline.Parent then
outline=Instance.new("SelectionBox") outline.Name="HitboxOutline" outline.Adornee=hrp outline.Color3=Color3.new(0,0,0) outline.LineThickness=0.05 outline.SurfaceTransparency=1 outline.Parent=Wksp hbOutlines[p]=outline
else outline.Adornee=hrp end
end
end
else
for _,o in pairs(hbOutlines) do if o then o:Destroy() end end hbOutlines={}
end

if playerEsp then
for _,p in ipairs(Plrs:GetPlayers()) do
local hc=false
if p~=LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid") then
local hum=p.Character.Humanoid if hum.Health>0 then hc=true local char=p.Character local hrp=char.HumanoidRootPart local ct=espContainers[p]
if not ct or not ct.Parent then
ct=Instance.new("Model") ct.Name="PlayerEsp_Container"
local hl=Instance.new("Highlight") hl.Name="PlayerHighlight" hl.Adornee=char hl.FillTransparency=1 hl.OutlineColor=boxColor hl.OutlineTransparency=0 hl.Parent=ct
local box=Instance.new("BoxHandleAdornment") box.Name="BoxOutline" box.Size=Vector3.new(2.4,4.8,1.5) box.Adornee=hrp box.AlwaysOnTop=true box.ZIndex=5 box.Transparency=0.65 box.Color3=boxColor box.Parent=ct
ct.Parent=Wksp espContainers[p]=ct
else local hl=ct:FindFirstChild("PlayerHighlight") if hl and hl.Adornee~=char then hl.Adornee=char end end
end end
if not hc then if espContainers[p] then espContainers[p]:Destroy() espContainers[p]=nil end end
end
else for _,ct in pairs(espContainers) do if ct then ct:Destroy() end end espContainers={} end
end)

Tv:CreateToggle({Name="Fullbright",CurrentValue=false,Callback=function(v) fbEn=v while fbEn and task.wait(0.5) do Lght.Brightness=2 Lght.ClockTime=14 Lght.FogEnd=100000 Lght.GlobalShadows=false end if not fbEn then Lght.GlobalShadows=true end end})
Tv:CreateToggle({Name="No Fog",CurrentValue=false,Callback=function(v) nfEn=v while nfEn and task.wait(1) do Lght.FogEnd=999999 for _,c in ipairs(Lght:GetChildren()) do if c:IsA("Atmosphere") then c.Density=0 end end end end})

local gGlassObj
gGlassObj=Tu:CreateToggle({Name="Ghost Glass Tool Toggle",CurrentValue=false,Callback=function(v)
local bp,c=LP:FindFirstChild("Backpack"),LP.Character
if v then
if bp and c and not (bp:FindFirstChild("Ghost Glass Tool") or c:FindFirstChild("Ghost Glass Tool")) then
local t=Instance.new("Tool") t.Name="Ghost Glass Tool" t.RequiresHandle=false t.Parent=bp
local lTgt,lTime,curBox,rConn,gParts=nil,0,nil,nil,{}
t.Activated:Connect(function()
local target=getTgt()
if target and target:IsA("BasePart") and target.Anchored and target.CanCollide and not target:IsDescendantOf(LP.Character or Wksp) then
local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
if not (hrp and target.Position.Y<hrp.Position.Y and ((target.Position*Vector3.new(1,0,1))-(hrp.Position*Vector3.new(1,0,1))).Magnitude<5) then
local cTime=tick()
if lTgt==target and (cTime-lTime)<0.4 then
if curBox then curBox:Destroy() curBox=nil end if rConn then rConn:Disconnect() rConn=nil end
target.Transparency,target.CanCollide=0.65,false table.insert(gParts,target) lTgt=nil
else
if curBox then curBox:Destroy() curBox=nil end if rConn then rConn:Disconnect() rConn=nil end
lTgt,lTime=target,cTime
curBox=Instance.new("SelectionBox") curBox.Adornee=target curBox.LineThickness=0.08 curBox.SurfaceTransparency=1 curBox.Parent=target
rConn=RunS.RenderStepped:Connect(function() if curBox and curBox.Parent then curBox.Color3=Color3.fromHSV(tick()%5/5,1,1) end end)
end
end
end
end)
t.Unequipped:Connect(function() if curBox then curBox:Destroy() curBox=nil end if rConn then rConn:Disconnect() rConn=nil end lTgt=nil end)
getgenv().CleanGG=function()
if curBox then curBox:Destroy() curBox=nil end if rConn then rConn:Disconnect() rConn=nil end
for _,p in ipairs(gParts) do if p and p.Parent then p.Transparency,p.CanCollide=0,true end end gParts={}
if t and t.Parent then t:Destroy() end local eq=c and c:FindFirstChild("Ghost Glass Tool") if eq then eq:Destroy() end
end
end
else if getgenv().CleanGG then getgenv().CleanGG() getgenv().CleanGG=nil end end
end})

local function onDeath() if getgenv().CleanGG then getgenv().CleanGG() getgenv().CleanGG=nil end pcall(function() gGlassObj:Set(false) end) end
LP.CharacterAdded:Connect(function(nc) local h=nc:WaitForChild("Humanoid",5) if h then h.Died:Connect(onDeath) end end)
if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.Died:Connect(onDeath) end

Tu:CreateButton({Name="Fly Menu Loader",Callback=function() pcall(function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Flyv2-30617"))() end) end})
Tu:CreateButton({Name="Get TP Tool",Callback=function() local bp,c=LP:FindFirstChild("Backpack"),LP.Character if bp and c and not (bp:FindFirstChild("TP Tool") or c:FindFirstChild("TP Tool")) then local t=Instance.new("Tool") t.Name="TP Tool" t.RequiresHandle=false t.Parent=bp local m=LP:GetMouse() t.Activated:Connect(function() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp and m.Target then hrp.CFrame=CFrame.new(m.Hit.Position+Vector3.new(0,3,0)) end end) end end})
Tu:CreateButton({Name="Get Rainbow Platform Tool",Callback=function() local bp,c=LP:FindFirstChild("Backpack"),LP.Character if bp and c and not bp:FindFirstChild("Rainbow Platform") then local t=Instance.new("Tool") t.Name="Rainbow Platform" t.RequiresHandle=false t.Parent=bp local plat,conn t.Equipped:Connect(function() conn=RunS.RenderStepped:Connect(function() local cc=LP.Character if cc and cc:FindFirstChild("HumanoidRootPart") then if not plat or not plat.Parent then plat=Instance.new("Part") plat.Size=Vector3.new(3,0.5,3) plat.Anchored=true plat.Transparency=0.2 plat.Parent=Wksp end plat.Color=Color3.fromHSV(tick()%5/5,1,1) plat.Position=cc.HumanoidRootPart.Position-Vector3.new(0,3.2,0) end end) end) local function cl() if conn then conn:Disconnect() conn=nil end if plat then plat:Destroy() plat=nil end end t.Unequipped:Connect(cl) t.AncestryChanged:Connect(function(_,p) if not p then cl() end end) end end})

Ta:CreateToggle({Name="Anti-AFK",CurrentValue=true,Callback=function(v) aAfk=v end})
LP.Idled:Connect(function() if aAfk then VUsr:CaptureController() VUsr:ClickButton2(Vector2.new()) end end)
Ta:CreateToggle({Name="Anti-Slow",CurrentValue=true,Callback=function(v) aSlw=v while aSlw and task.wait(0.5) do local c=LP.Character if c and c:FindFirstChild("Humanoid") and c.Humanoid.WalkSpeed<16 and not sEn then c.Humanoid.WalkSpeed=16 end end end})
Ta:CreateToggle({Name="Anti-Ragdoll",CurrentValue=false,Callback=function(v) aRag=v while aRag and task.wait(0.1) do local c=LP.Character if c and c:FindFirstChild("Humanoid") then local s=c.Humanoid:GetState() if s==Enum.HumanoidStateType.FallingDown or s==Enum.HumanoidStateType.Ragdoll then c.Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp) end end end end})
Ta:CreateButton({Name="Load Anti-Fling Script",Callback=function() pcall(function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-anti-fling-script-241540"))() end) end})

Ts:CreateButton({Name="Rejoin Current Server",Callback=function() Tele:TeleportToPlaceInstance(game.PlaceId,game.JobId,LP) end})
Ts:CreateButton({Name="Random Server Hop",Callback=function() pcall(function() local r=Http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100")) for _,s in ipairs(r.data) do if s.id~=game.JobId and s.playing<s.maxPlayers then Tele:TeleportToPlaceInstance(game.PlaceId,s.id,LP) break end end end) end})

local sDrop,fSrv=nil,{}
sDrop=Ts:CreateDropdown({Name="Select Low-Player Server",Options={"Click 'FetchServers' first"},CurrentOption="Click 'Fetch Servers' first",Callback=function(o) local j=fSrv[type(o)=="table" and o[1] or o] if j then Tele:TeleportToPlaceInstance(game.PlaceId,j,LP) end end})
Ts:CreateButton({Name="Fetch Small Servers List",Callback=function()
fSrv={} local opts={}
pcall(function()
local r=Http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=50"))
table.sort(r.data,function(a,b) return a.playing<b.playing end)
for _,s in ipairs(r.data) do if s.id~=game.JobId and s.playing<s.maxPlayers then local l=string.format("Players: %d/%d",s.playing,s.maxPlayers) fSrv[l]=s.id table.insert(opts,l) end end
end)
sDrop:Refresh(#opts>0 and opts or {"No other servers found"},true)
end})

R:LoadConfiguration()

