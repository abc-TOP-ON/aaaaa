-- 1. استدعاء الخدمات الأساسية وإعداد الحاوية
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- منع إعادة التشغيل
if _G.ABCLoadingRunning then return end
_G.ABCLoadingRunning = true

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ABC_Premium_Loading"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.IgnoreGuiInset = true

-- 2. الخلفية السينمائية الداكنة مع تدرج لوني
local Background = Instance.new("Frame")
Background.Size = UDim2.new(1, 0, 1, 0)
Background.BackgroundColor3 = Color3.fromRGB(5, 5, 8)
Background.BorderSizePixel = 0
Background.Parent = ScreenGui

-- إضافة تدرج لوني فاخر
local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(5, 5, 8)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 10, 30))
})
gradient.Parent = Background

-- [إضافة 10x]: نظام الجزيئات المتحركة في الخلفية (مكثف)
local function createParticle()
    local particle = Instance.new("Frame")
    local size = math.random(1, 3) -- تصغير حجم الجزيئات
    particle.Size = UDim2.new(0, size, 0, size)
    particle.Position = UDim2.new(math.random(), 0, -0.1, 0)
    
    -- ألوان متعددة للجزيئات
    local colors = {
        Color3.fromRGB(138, 43, 226),
        Color3.fromRGB(100, 50, 200),
        Color3.fromRGB(180, 80, 255),
        Color3.fromRGB(80, 30, 180)
    }
    particle.BackgroundColor3 = colors[math.random(1, #colors)]
    particle.BackgroundTransparency = math.random(2, 6) / 10
    particle.BorderSizePixel = 0
    particle.Parent = Background
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(1, 0)
    UICorner.Parent = particle

    local speed = math.random(5, 10) -- زيادة السرعة
    local tween = TweenService:Create(particle, TweenInfo.new(speed, Enum.EasingStyle.Linear), {
        Position = UDim2.new(particle.Position.X.Scale, 0, 1.1, 0),
        BackgroundTransparency = 1
    })
    tween:Play()
    tween.Completed:Connect(function() particle:Destroy() end)
end

-- توليد الجزيئات بكثافة عالية
local particleLoop = task.spawn(function()
    while task.wait(0.08) do -- تقليل وقت الانتظار = جزيئات أكثر
        createParticle()
        -- توليد جزيئين أحياناً
        if math.random(1, 3) == 1 then
            createParticle()
        end
    end
end)

-- 3. الشعار العلوي الدائري المصغر (ABC)
local LogoCircle = Instance.new("Frame")
LogoCircle.Size = UDim2.new(0, 75, 0, 75)
LogoCircle.Position = UDim2.new(0.5, -37.5, 0.14, 0)
LogoCircle.BackgroundColor3 = Color3.fromRGB(15, 10, 25)
LogoCircle.Parent = Background

local LogoCorner = Instance.new("UICorner")
LogoCorner.CornerRadius = UDim.new(1, 0)
LogoCorner.Parent = LogoCircle

local LogoStroke = Instance.new("UIStroke")
LogoStroke.Color = Color3.fromRGB(138, 43, 226)
LogoStroke.Thickness = 2.5
LogoStroke.Parent = LogoCircle

local LogoText = Instance.new("TextLabel")
LogoText.Size = UDim2.new(1, 0, 1, 0)
LogoText.Text = "ABC"
LogoText.TextColor3 = Color3.fromRGB(180, 100, 255)
LogoText.TextSize = 24
LogoText.Font = Enum.Font.FredokaOne
LogoText.BackgroundTransparency = 1
LogoText.Parent = LogoCircle

-- 4. صورة اللاعب (الأفاتار) - تصغير الحجم
local AvatarCircle = Instance.new("ImageLabel")
AvatarCircle.Size = UDim2.new(0, 55, 0, 55) -- من 90 إلى 55
AvatarCircle.Position = UDim2.new(0.5, -27.5, 0.36, 0) -- تعديل الموقع
AvatarCircle.BackgroundColor3 = Color3.fromRGB(12, 12, 18)
AvatarCircle.Parent = Background

local userId = LocalPlayer.UserId
AvatarCircle.Image = Players:GetUserThumbnailAsync(userId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size150x150)

local AvatarCorner = Instance.new("UICorner")
AvatarCorner.CornerRadius = UDim.new(1, 0)
AvatarCorner.Parent = AvatarCircle

local AvatarStroke = Instance.new("UIStroke")
AvatarStroke.Color = Color3.fromRGB(100, 60, 160)
AvatarStroke.Thickness = 2
AvatarStroke.Parent = AvatarCircle

-- 5. النصوص والترتيب الفاخر (ABC TOP ON)
local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, 0, 0, 35)
TitleText.Position = UDim2.new(0, 0, 0.56, 0) -- تعديل الموقع
TitleText.Text = "ABC TOP ON"
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextSize = 28
TitleText.Font = Enum.Font.GothamBold
TitleText.BackgroundTransparency = 1
TitleText.Parent = Background

-- إضافة توهج نصي خفيف تحت الاسم الرئيسي
local TitleGlow = Instance.new("UIStroke")
TitleGlow.Color = Color3.fromRGB(138, 43, 226)
TitleGlow.Thickness = 1
TitleGlow.Transparency = 0.5
TitleGlow.Parent = TitleText

local WelcomeText = Instance.new("TextLabel")
WelcomeText.Size = UDim2.new(1, 0, 0, 30)
WelcomeText.Position = UDim2.new(0, 0, 0.63, 0) -- تعديل الموقع
WelcomeText.Text = "Welcome, " .. LocalPlayer.Name
WelcomeText.TextColor3 = Color3.fromRGB(160, 160, 170)
WelcomeText.TextSize = 16
WelcomeText.Font = Enum.Font.Gotham
WelcomeText.BackgroundTransparency = 1
WelcomeText.Parent = Background

-- [إضافة 10x]: نصوص الحالة المتغيرة ديناميكياً
local StatusText = Instance.new("TextLabel")
StatusText.Size = UDim2.new(1, 0, 0, 20)
StatusText.Position = UDim2.new(0, 0, 0.71, 0) -- تعديل الموقع
StatusText.Text = "Initializing..."
StatusText.TextColor3 = Color3.fromRGB(138, 43, 226)
StatusText.TextSize = 13
StatusText.Font = Enum.Font.Code
StatusText.BackgroundTransparency = 1
StatusText.Parent = Background

-- 6. شريط التحميل المطور وعداد النسبة المئوية
local LoadingBarBg = Instance.new("Frame")
LoadingBarBg.Size = UDim2.new(0, 280, 0, 4)
LoadingBarBg.Position = UDim2.new(0.5, -140, 0.80, 0)
LoadingBarBg.BackgroundColor3 = Color3.fromRGB(25, 20, 35)
LoadingBarBg.BorderSizePixel = 0
LoadingBarBg.Parent = Background

local LoadingBarFill = Instance.new("Frame")
LoadingBarFill.Size = UDim2.new(0, 0, 1, 0)
LoadingBarFill.BackgroundColor3 = Color3.fromRGB(160, 50, 255)
LoadingBarFill.BorderSizePixel = 0
LoadingBarFill.Parent = LoadingBarBg

-- [إضافة 10x]: نص النسبة المئوية (%0 -> %100)
local PercentageText = Instance.new("TextLabel")
PercentageText.Size = UDim2.new(0, 50, 0, 20)
PercentageText.Position = UDim2.new(0.5, -25, 0.83, 0)
PercentageText.Text = "0%"
PercentageText.TextColor3 = Color3.fromRGB(200, 200, 210)
PercentageText.TextSize = 14
PercentageText.Font = Enum.Font.GothamMedium
PercentageText.BackgroundTransparency = 1
PercentageText.Parent = Background

-- 7. نظام كتابة النصوص حرف حرف (Typewriter)
local function typeWriter(textLabel, text, speed)
    textLabel.Text = ""
    for i = 1, #text do
        textLabel.Text = string.sub(text, 1, i)
        task.wait(speed)
    end
end

-- 8. المؤشر المتحرك (dots)
task.spawn(function()
    local dots = ""
    while task.wait(0.3) do
        dots = (#dots >= 3) and "" or dots .. "."
        if StatusText.Text:find("...") then
            StatusText.Text = StatusText.Text:gsub("%.%.%.", "")
        end
        StatusText.Text = StatusText.Text .. dots
    end
end)

-- 9. البرمجة والأنيميشن الاحترافي الشامل
task.spawn(function()
    -- أنيميشن نبض الشعار والتوهج المستمر
    local pulseInfo = TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    TweenService:Create(LogoStroke, pulseInfo, {Thickness = 4.5, Color = Color3.fromRGB(210, 80, 255)}):Play()
    TweenService:Create(AvatarStroke, pulseInfo, {Thickness = 3.5, Color = Color3.fromRGB(140, 80, 255)}):Play()
    
    -- نظام محاكاة تغيير نصوص الحالة (مع تأثير الكتابة)
    local statuses = {
        {time = 0.8, text = "Bypassing Anticheat"},
        {time = 1.8, text = "Loading ABC TOP ON Framework"},
        {time = 2.8, text = "Injecting Modules & Scripts"},
        {time = 3.6, text = "Ready! Booting UI"}
    }
    
    task.spawn(function()
        for _, status in ipairs(statuses) do
            task.wait(status.time)
            typeWriter(StatusText, status.text, 0.05)
        end
    end)
    
    -- تشغيل حركة تعبئة شريط التحميل (5 ثوانٍ)
    local loadTime = 5 -- تغيير إلى 5 ثواني
    local loadInfo = TweenInfo.new(loadTime, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local loadTween = TweenService:Create(LoadingBarFill, loadInfo, {Size = UDim2.new(1, 0, 1, 0)})
    loadTween:Play()
    
    -- تحديث عداد النسبة المئوية بدقة وسلاسة
    local startTime = os.clock()
    local connection
    connection = RunService.RenderStepped:Connect(function()
        local elapsed = os.clock() - startTime
        local progress = math.clamp(elapsed / loadTime, 0, 1)
        local smoothProgress = progress * (2 - progress) 
        PercentageText.Text = math.floor(smoothProgress * 100) .. "%"
        
        if progress >= 1 then
            connection:Disconnect()
        end
    end)
    
    loadTween.Completed:Wait()
    task.wait(0.4)
    
    -- إيقاف توليد الجزيئات
    task.cancel(particleLoop)
    
    -- تشغيل صوت النجاح (اختياري)
    local successSound = Instance.new("Sound")
    successSound.SoundId = "rbxassetid://9120396384"
    successSound.Volume = 0.3
    successSound.Parent = Background
    successSound:Play()
    
    -- أنيميشن اختفاء الواجهة السينمائي (Fade Out)
    local fadeInfo = TweenInfo.new(0.8, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local fadeBackground = TweenService:Create(Background, fadeInfo, {BackgroundTransparency = 1})
    
    -- تصفير وتلاشي كافة العناصر
    for _, item in pairs(Background:GetChildren()) do
        if item:IsA("TextLabel") then
            TweenService:Create(item, fadeInfo, {TextTransparency = 1}):Play()
            local stroke = item:FindFirstChildOfClass("UIStroke")
            if stroke then TweenService:Create(stroke, fadeInfo, {Transparency = 1}):Play() end
        elseif item:IsA("ImageLabel") then
            TweenService:Create(item, fadeInfo, {ImageTransparency = 1}):Play()
            local stroke = item:FindFirstChildOfClass("UIStroke")
            if stroke then TweenService:Create(stroke, fadeInfo, {Transparency = 1}):Play() end
        elseif item:IsA("Frame") then
            TweenService:Create(item, fadeInfo, {BackgroundTransparency = 1}):Play()
            local stroke = item:FindFirstChildOfClass("UIStroke")
            if stroke then TweenService:Create(stroke, fadeInfo, {Transparency = 1}):Play() end
        end
    end
    
    fadeBackground:Play()
    fadeBackground.Completed:Wait()
    task.wait(0.2)
    ScreenGui:Destroy() -- تدمير الواجهة بالكامل
    _G.ABCLoadingRunning = false -- إعادة تعيين المتغير
end)
