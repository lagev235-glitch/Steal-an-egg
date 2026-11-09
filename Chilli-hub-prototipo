-- =========================================================
-- Chilli V8 - STRIPPED: APENAS ABA 2 (STEAL) E SEUS SISTEMAS
-- Todas as outras abas da sidebar são escondidas/desativadas.
-- Sistemas de steal vêm do próprio Venyx (VENYX_URL).
-- =========================================================

if not math.clamp then
    math.clamp = function(v, a, b)
        return math.max(a, math.min(b, v))
    end
end

local NEW_TITLE = "Chilli V8 My Ass"
local LOGO_ID = "rbxassetid://74441610995894"
local STEAL_TAB_ICON_ID = "rbxassetid://91536384271341"
local WALLPAPER_IMAGE_ID = "rbxassetid://90615151648668"
local VENYX_URL = "https://raw.githubusercontent.com/tienkhanh1/Venyx/main/VenyxHub"

local c3, u2, u2s, v2, csk = Color3.fromRGB, UDim2.new, UDim2.fromScale, Vector2.new, ColorSequenceKeypoint.new

local env = (getgenv and getgenv()) or _G
env.__ChilliV5State = env.__ChilliV5State or {SessionId = 0, Language = "EN", ActiveHub = "CHILLI", TrackedTexts = {}, StartClock = os.clock()}
local State = env.__ChilliV5State
State.Language = "EN"
State.SessionId = (State.SessionId or 0) + 1
State.ActiveHub = "CHILLI"
State.StartClock = State.StartClock or os.clock()

local SID = State.SessionId
local function isAlive() return State.SessionId == SID end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local GuiService = game:GetService("GuiService")

local function getScanRoots()
    local r, seen = {}, {}
    local function addRoot(inst)
        if inst and not seen[inst] then seen[inst] = true; table.insert(r, inst) end
    end
    if gethui then pcall(function() addRoot(gethui()) end) end
    addRoot(CoreGui)
    addRoot(LocalPlayer and LocalPlayer:FindFirstChildOfClass("PlayerGui"))
    return r
end

local IGNORED_GUIS = {RobloxGui=true, TopBarApp=true, TouchGui=true, Chat=true, BubbleChat=true, Freecam=true, BackpackGui=true, PlayerList=true, PurchasePrompt=true, ChilliV5_NoticeGui=true, ChilliV8_MasterIslandGui=true, CoreScriptsRootProvider=true, ExperienceChat=true, SettingsClippingShield=true, InGameMenu=true, TeleportGui=true, RobloxNetworkPauseNotification=true}

local function isVenyxScreenGui(gui)
    if not gui or not gui:IsA("ScreenGui") or IGNORED_GUIS[gui.Name] then return false end
    if gui == State.ActiveVenyxGui or gui:GetAttribute("__IsChilliVenyxGui") or gui:GetAttribute("__ChilliV8Patched") then return true end
    for _, d in ipairs(gui:GetDescendants()) do
        if d.Name == "ChilliHeaderOverlay" or d.Name == "ChilliProWindowStroke" then
            gui:SetAttribute("__IsChilliVenyxGui", true); return true
        end
        if (d:IsA("TextLabel") or d:IsA("TextButton")) and not d:FindFirstAncestorOfClass("ScrollingFrame") then
            local t = (d.Text or ""):match("^%s*(.-)%s*$")
            if t == "VENYX" or t == NEW_TITLE then
                gui:SetAttribute("__IsChilliVenyxGui", true); return true
            end
        end
    end
    return false
end

local function isVenyxUiPresent()
    for _, root in ipairs(getScanRoots()) do
        for _, ch in ipairs(root:GetChildren()) do
            if ch:IsA("ScreenGui") and not IGNORED_GUIS[ch.Name] and ch.Enabled ~= false then
                for _, d in ipairs(ch:GetDescendants()) do
                    if (d:IsA("TextLabel") or d:IsA("TextButton")) and not d:FindFirstAncestorOfClass("ScrollingFrame") then
                        if d.Text == "VENYX" or d.Text == NEW_TITLE then return true end
                    end
                end
            end
        end
    end
    return false
end

local function setupSafeTeleportHook()
    local rq = env.__ChilliOrigQueue or env.queue_on_teleport or env.queueonteleport or queue_on_teleport or queueonteleport or (syn and syn.queue_on_teleport)
    if not rq then return end
    env.__ChilliOrigQueue = rq
    local boot = '--[[CHILLI_V8_AUTHORIZED_TELEPORT]]\ntask.spawn(function() if not game:IsLoaded() then game.Loaded:Wait() end loadstring(game:HttpGet("' .. VENYX_URL .. '"))() end)'
    local safeLock = function(c)
        pcall(function()
            if tostring(c or ""):find("CHILLI_V8_AUTHORIZED_TELEPORT", 1, true) then
                rq(c)
            else
                if env.__ChilliBootQueuedSession ~= SID then env.__ChilliBootQueuedSession = SID; rq(boot) end
            end
        end)
    end
    pcall(function()
        env.queue_on_teleport = safeLock
        env.queueonteleport = safeLock
        if env.__ChilliBootQueuedSession ~= SID then env.__ChilliBootQueuedSession = SID; pcall(rq, boot) end
    end)
end

local function safeHttpGet(url)
    local ok, res = pcall(function() return game:HttpGet(url) end)
    if ok and type(res) == "string" and #res > 10 then return res end
    local req = (syn and syn.request) or (http and http.request) or http_request or request
    if req then
        local rOk, rRes = pcall(req, {Url = url, Method = "GET"})
        if rOk and rRes and type(rRes.Body) == "string" and #rRes.Body > 10 then return rRes.Body end
    end
    return nil
end

local function launchVenyxBase()
    if isVenyxUiPresent() then return end
    if env.__VenyxLaunchingNow and (os.clock() - (env.__VenyxLaunchClock or 0) < 6) then return end
    env.__VenyxLaunchingNow = true
    env.__VenyxLaunchClock = os.clock()
    task.spawn(function()
        if not game:IsLoaded() then game.Loaded:Wait() end
        local rawCode = safeHttpGet(VENYX_URL)
        if rawCode then
            local fn = loadstring(rawCode)
            if type(fn) == "function" then pcall(fn) end
        end
        env.__VenyxLaunchingNow = false
        pcall(setupSafeTeleportHook)
    end)
end

launchVenyxBase()

local function mk(cls, props, parent)
    local inst = Instance.new(cls)
    for k, v in pairs(props) do inst[k] = v end
    if parent then inst.Parent = parent end
    return inst
end

local function addGrad(g) if g then pcall(function() g.Rotation = 35 end) end; return g end

local RAINBOW = ColorSequence.new({csk(0.00, c3(255, 45, 85)), csk(0.17, c3(255, 155, 35)), csk(0.33, c3(255, 235, 45)), csk(0.50, c3(45, 255, 145)), csk(0.67, c3(45, 195, 255)), csk(0.83, c3(185, 85, 255)), csk(1.00, c3(255, 45, 85))})
local LIQUID_GLASS_COLOR = ColorSequence.new({csk(0.00, c3(145, 190, 235)), csk(0.20, c3(22, 30, 48)), csk(0.78, c3(12, 16, 28)), csk(1.00, c3(48, 82, 132))})
local LIQUID_GLASS_TRANS = NumberSequence.new({NumberSequenceKeypoint.new(0.00, 0.18), NumberSequenceKeypoint.new(0.22, 0.28), NumberSequenceKeypoint.new(0.78, 0.26), NumberSequenceKeypoint.new(1.00, 0.20)})
local FINDER_BORDER_NEON_GRAD = ColorSequence.new({csk(0.00, c3(255, 65, 125)), csk(0.20, c3(255, 195, 45)), csk(0.40, c3(55, 255, 165)), csk(0.60, c3(45, 215, 255)), csk(0.80, c3(195, 95, 255)), csk(1.00, c3(255, 65, 125))})

local function stripRichTags(s) if type(s) ~= "string" then return "" end; return (s:gsub("<[^>]+>", ""):match("^%s*(.-)%s*$") or "") end

local NAME_MAP = {["VENYX"] = NEW_TITLE, ["Venyx"] = "Chilli Hub", ["Chilli Hub V5"] = NEW_TITLE, ["Chilli V5 |"] = NEW_TITLE}
local function translateText(raw)
    if not raw or raw == "" then return raw end
    if raw:find("Venyx SAE", 1, true) then raw = raw:gsub("Venyx SAE", "Chilli SAE")
    elseif NAME_MAP[raw] then raw = NAME_MAP[raw] end
    return raw
end

local ISLAND_SFX_CFG = {
    open = {id = "rbxassetid://6895079853", vol = 0.75, spd = 1.06},
    close = {id = "rbxassetid://6895079853", vol = 0.55, spd = 0.86},
    tap = {id = "rbxassetid://6026984224", vol = 0.70, spd = 1.12},
    action = {id = "rbxassetid://4590662766", vol = 0.72, spd = 1.08},
}
State.CachedIslandSfx = State.CachedIslandSfx or {}
local function getIslandSound(mode)
    local key = mode or "tap"
    local cfg = ISLAND_SFX_CFG[key] or ISLAND_SFX_CFG.tap
    local snd = State.CachedIslandSfx[key]
    if not (snd and snd.Parent) then
        snd = Instance.new("Sound")
        snd.Name = "ChilliV8_Sfx_" .. key
        snd.SoundId = cfg.id
        snd.Volume = cfg.vol
        snd.PlaybackSpeed = cfg.spd
        snd.Parent = SoundService
        State.CachedIslandSfx[key] = snd
    end
    return snd
end
local function playIslandSfx(mode)
    pcall(function() local snd = getIslandSound(mode); snd.TimePosition = 0; snd:Play() end)
end

-- ============ SIDEBAR: ONLY SECOND TAB (index 2 = Steal) ============
local TARGET_TAB_INDEX = 2

local function hideAllOtherTabs(navButtons)
    if not navButtons then return end
    for idx, btn in ipairs(navButtons) do
        if idx ~= TARGET_TAB_INDEX then
            if btn then
                btn.Visible = false
                btn.Active = false
                for _, ch in ipairs(btn:GetDescendants()) do
                    if ch:IsA("GuiObject") then pcall(function() ch.Visible = false end) end
                end
                if not btn:GetAttribute("__Tab2OnlyLocked") then
                    btn:SetAttribute("__Tab2OnlyLocked", true)
                    pcall(function()
                        btn:GetPropertyChangedSignal("Visible"):Connect(function()
                            if btn.Visible then btn.Visible = false end
                        end)
                    end)
                end
            end
        end
    end
end

local function forceSelectTab2(navButtons)
    if not navButtons or not navButtons[TARGET_TAB_INDEX] then return end
    local btn = navButtons[TARGET_TAB_INDEX]
    if not btn or not btn.Parent or not btn.Visible then return end
    -- Simula clique na aba Steal
    pcall(function()
        if firesignal and btn.MouseButton1Click then firesignal(btn.MouseButton1Click) end
    end)
    pcall(function()
        if btn.Activated then btn.Activated:Fire() end
    end)
end

local function setupIsland(statusLbl, venyxGui)
    local pill = statusLbl.Parent
    if not (pill and pill:IsA("GuiObject")) then return end
    statusLbl:SetAttribute("__IslandDone", true)
    State.ActiveStatusLbl = statusLbl

    for _, ch in ipairs(pill:GetChildren()) do
        if ch:IsA("UIGradient") and ch.Name ~= "LiquidGlassSheen" then
            pcall(ch.Destroy, ch)
        elseif ch.Name == "IslandClickHitbox" or ch.Name == "ChilliHeaderLogo" or ch.Name == "ChilliCircleDisc" then
            pcall(ch.Destroy, ch)
        elseif (ch:IsA("Frame") or ch:IsA("ImageLabel") or ch:IsA("ImageButton")) and ch ~= statusLbl and ch.Name ~= "IslandExpandHint" then
            ch.Visible = false
        end
    end

    pill.BackgroundColor3 = Color3.new(1, 1, 1)
    pill.BackgroundTransparency = 0
    pill.Size = u2(0, 0, 0, 38)
    pill.AutomaticSize = Enum.AutomaticSize.X

    local glassGrad = pill:FindFirstChild("LiquidGlassSheen") or mk("UIGradient", {Name = "LiquidGlassSheen", Rotation = 90, Color = LIQUID_GLASS_COLOR, Transparency = LIQUID_GLASS_TRANS}, pill)
    glassGrad.Rotation = 90; glassGrad.Color = LIQUID_GLASS_COLOR; glassGrad.Transparency = LIQUID_GLASS_TRANS

    local pillLayout = pill:FindFirstChildOfClass("UIListLayout")
    if not pillLayout then
        mk("UIListLayout", {Parent = pill, FillDirection = Enum.FillDirection.Horizontal, HorizontalAlignment = Enum.HorizontalAlignment.Center, VerticalAlignment = Enum.VerticalAlignment.Center, Padding = UDim.new(0, 8), SortOrder = Enum.SortOrder.LayoutOrder})
    end

    local pad = pill:FindFirstChildOfClass("UIPadding") or mk("UIPadding", {}, pill)
    pad.PaddingLeft = UDim.new(0, 10); pad.PaddingRight = UDim.new(0, 10)
    pad.PaddingTop = UDim.new(0, 2); pad.PaddingBottom = UDim.new(0, 2)

    local leftBadge = pill:FindFirstChild("IslandLogoBadge")
    if not leftBadge then
        leftBadge = mk("Frame", {Name = "IslandLogoBadge", LayoutOrder = -10, Size = u2(0, 28, 0, 28), BackgroundColor3 = c3(0, 0, 0), BorderSizePixel = 0, ClipsDescendants = true, Active = false, ZIndex = 10000}, pill)
        mk("UICorner", {CornerRadius = UDim.new(1, 0)}, leftBadge)
        local lbImg = mk("ImageLabel", {Name = "LogoImg", AnchorPoint = v2(0.5, 0.5), Position = u2s(0.5, 0.5), Size = u2s(1, 1), BackgroundTransparency = 1, Image = LOGO_ID, ScaleType = Enum.ScaleType.Crop, Active = false, ZIndex = 10001}, leftBadge)
        mk("UICorner", {CornerRadius = UDim.new(1, 0)}, lbImg)
        mk("UIStroke", {Thickness = 1.2, Color = c3(65, 195, 255), ApplyStrokeMode = Enum.ApplyStrokeMode.Border}, lbImg)
    end

    local hint = pill:FindFirstChild("IslandExpandHint")
    if not hint then
        hint = mk("TextLabel", {Name = "IslandExpandHint", LayoutOrder = 99, Size = u2(0, 24, 0, 22), BackgroundColor3 = c3(22, 32, 54), BackgroundTransparency = 0.18, BorderSizePixel = 0, Text = "▼", Font = Enum.Font.GothamBlack, TextSize = 10, TextColor3 = c3(205, 238, 255), Active = true, ZIndex = 10000}, pill)
        mk("UICorner", {CornerRadius = UDim.new(1, 0)}, hint)
        mk("UIStroke", {Thickness = 1.1, Color = c3(135, 215, 255), Transparency = 0.25}, hint)
    end

    statusLbl.RichText = false
    statusLbl.LayoutOrder = 2
    statusLbl.AutomaticSize = Enum.AutomaticSize.XY
    statusLbl.Font = Enum.Font.GothamBold
    statusLbl.TextSize = 13
    statusLbl.TextColor3 = Color3.new(1, 1, 1)
    statusLbl.TextTransparency = 0

    local function syncStatus()
        if statusLbl:GetAttribute("__Updating") then return end
        statusLbl:SetAttribute("__Updating", true)
        local cur = (statusLbl.Text or ""):match("^%s*(.-)%s*$") or ""
        if cur == "" then cur = "Idle" end
        statusLbl:SetAttribute("__LastTranslatedStatus", cur)
        statusLbl.Text = cur
        statusLbl:SetAttribute("__Updating", false)
    end
    State.RefreshIslandCallback = syncStatus
    syncStatus()

    if statusLbl:GetAttribute("__LockedStatusColor") ~= SID then
        statusLbl:SetAttribute("__LockedStatusColor", SID)
        statusLbl:GetPropertyChangedSignal("Text"):Connect(function()
            if isAlive() and not statusLbl:GetAttribute("__Updating") then syncStatus() end
        end)
    end

    for _, r in ipairs(getScanRoots()) do
        for _, d in ipairs(r:GetDescendants()) do
            if d.Name == "ChilliV5_ExpandedIsland" then pcall(d.Destroy, d) end
        end
    end

    pill.Parent = venyxGui
    pill.AnchorPoint = v2(0.5, 0)
    pill.ZIndex = 9999
    pill.Active = true
    pill.Visible = true
    pill.Position = u2(0.5, 0, 0, 6)

    if not pill:FindFirstChildOfClass("UICorner") then mk("UICorner", {CornerRadius = UDim.new(1, 0)}, pill) end
    mk("UIStroke", {Name = "IslandStroke", Thickness = 1.5, ApplyStrokeMode = Enum.ApplyStrokeMode.Border, Color = c3(55, 205, 255), Transparency = 0.12}, pill)

    local exp = mk("Frame", {Name = "ChilliV5_ExpandedIsland", AnchorPoint = v2(0.5, 0), Position = pill.Position, Size = u2(0, 312, 0, 70), BackgroundColor3 = Color3.new(1, 1, 1), BackgroundTransparency = 0.05, BorderSizePixel = 0, Visible = false, Active = true, ClipsDescendants = true, ZIndex = 10000}, venyxGui)
    mk("UICorner", {CornerRadius = UDim.new(0, 20)}, exp)
    mk("UIGradient", {Name = "LiquidGlassPanelGrad", Rotation = 90, Color = ColorSequence.new({csk(0.00, c3(34, 48, 78)), csk(0.22, c3(16, 22, 38)), csk(0.78, c3(10, 14, 24)), csk(1.00, c3(22, 34, 58))}), Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0.00, 0.06), NumberSequenceKeypoint.new(0.30, 0.10), NumberSequenceKeypoint.new(1.00, 0.06)})}, exp)
    mk("UIStroke", {Thickness = 1.6, Color = c3(85, 195, 255), Transparency = 0.14, ApplyStrokeMode = Enum.ApplyStrokeMode.Border}, exp)

    local expLogo = mk("Frame", {Position = u2(0, 10, 0, 7), Size = u2(0, 26, 0, 26), BackgroundColor3 = c3(0, 0, 0), BorderSizePixel = 0, ClipsDescendants = true, ZIndex = 10002}, exp)
    mk("UICorner", {CornerRadius = UDim.new(1, 0)}, expLogo)
    mk("ImageLabel", {AnchorPoint = v2(0.5, 0.5), Position = u2s(0.5, 0.5), Size = u2s(1, 1), BackgroundTransparency = 1, Image = LOGO_ID, ScaleType = Enum.ScaleType.Crop, ZIndex = 10003}, expLogo)

    local colBtn = mk("TextButton", {Size = u2(0, 28, 0, 22), Position = u2(1, -36, 0, 8), BackgroundColor3 = c3(26, 36, 58), BackgroundTransparency = 0.15, BorderSizePixel = 0, Text = "▲", Font = Enum.Font.GothamBold, TextSize = 10, TextColor3 = c3(195, 235, 255), ZIndex = 10003}, exp)
    mk("UICorner", {CornerRadius = UDim.new(1, 0)}, colBtn)

    local taskBox = mk("Frame", {Name = "IslandTaskDisplay", Size = u2(1, -20, 0, 28), Position = u2(0, 10, 0, 36), BackgroundColor3 = c3(20, 28, 46), BackgroundTransparency = 0.05, BorderSizePixel = 0, ZIndex = 10001}, exp)
    mk("UICorner", {CornerRadius = UDim.new(1, 0)}, taskBox)
    local taskLbl = mk("TextLabel", {Name = "TaskLabel", Size = u2(1, -16, 1, 0), Position = u2(0, 8, 0, 0), BackgroundTransparency = 1, Text = "⚡ " .. (statusLbl.Text or "Idle"), Font = Enum.Font.GothamBold, TextSize = 10.8, TextColor3 = Color3.new(1, 1, 1), TextTruncate = Enum.TextTruncate.AtEnd, ZIndex = 10002}, taskBox)

    local open, lastToggleClock = false, -10
    local function toggle()
        if os.clock() - lastToggleClock < 0.12 then return end
        lastToggleClock = os.clock()
        open = not open
        playIslandSfx(open and "open" or "close")
        if open then
            exp.Position = pill.Position
            exp.Size = u2(0, 240, 0, 36)
            exp.Visible = true
            pill.Visible = false
            TweenService:Create(exp, TweenInfo.new(0.12, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = u2(0, 312, 0, 70)}):Play()
        else
            local tw = TweenService:Create(exp, TweenInfo.new(0.12, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Size = u2(0, 240, 0, 36)})
            tw:Play()
            tw.Completed:Connect(function()
                if not open then exp.Visible = false; pill.Visible = true end
            end)
        end
    end
    if hint:IsA("GuiButton") then
        hint.MouseButton1Click:Connect(toggle)
    else
        hint.InputBegan:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then toggle() end
        end)
    end
    colBtn.MouseButton1Click:Connect(toggle)

    statusLbl:GetPropertyChangedSignal("Text"):Connect(function()
        if taskLbl and statusLbl.Text then taskLbl.Text = "⚡ " .. statusLbl.Text end
    end)

    local lastPillTouchClock = -10
    local function onIn(i)
        if i.UserInputType ~= Enum.UserInputType.MouseButton1 and i.UserInputType ~= Enum.UserInputType.Touch then return end
        if os.clock() - lastPillTouchClock < 0.08 then return end
        lastPillTouchClock = os.clock()
        local start = i.Position
        local startP = pill.Position
        local dragConn, upConn
        local hasMoved, finished = false, false
        local function finishInput()
            if finished then return end
            finished = true
            if dragConn then dragConn:Disconnect() end
            if upConn then upConn:Disconnect() end
            if not hasMoved and not open then toggle() end
        end
        dragConn = UserInputService.InputChanged:Connect(function(im)
            if im.UserInputType == Enum.UserInputType.MouseMovement or im.UserInputType == Enum.UserInputType.Touch then
                local d = im.Position - start
                if math.abs(d.X) > 10 or math.abs(d.Y) > 10 then
                    hasMoved = true
                    local newPos = u2(startP.X.Scale, startP.X.Offset + d.X, startP.Y.Scale, startP.Y.Offset + d.Y)
                    pill.Position = newPos
                    if exp and exp.Visible then exp.Position = newPos end
                end
            end
        end)
        upConn = UserInputService.Input
