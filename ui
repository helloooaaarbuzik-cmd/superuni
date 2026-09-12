								--==============================================================
-- ⚡ SUPERUNI V11 FULL
--
-- PHONE = NEON UI
-- PC = LARGE CLASSIC UI
--
-- 120 FUNCTIONS
-- UNLIMITED TELEPORT POINTS
-- CUSTOM HOTKEYS
-- SWIPE CATEGORIES
--
-- FRIEND SYSTEM:
-- MIDDLE MOUSE / MOUSE WHEEL CLICK ON PLAYER
-- = ADD / REMOVE FRIEND
--
-- FRIENDS:
-- GREEN ESP
-- GREEN NAME
-- GREEN DISTANCE
-- AUTO AIM IGNORES FRIENDS
--
-- PC DEFAULT:
-- TAB = MENU
-- F   = FLY
-- 8 / NUMPAD8 = FLY UP
-- 2 / NUMPAD2 = FLY DOWN
-- N   = NOCLIP
-- Q   = AUTO AIM
-- E   = ESP
-- V   = SPEED
-- H   = HEAL
--
-- LocalScript:
-- StarterPlayer > StarterPlayerScripts
--
-- PASSWORD:
-- Premium_6339466
--==============================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")
local TweenService = game:GetService("TweenService")

local LP = Players.LocalPlayer
local PG = LP:WaitForChild("PlayerGui")
local Mouse = LP:GetMouse()

local Camera = workspace.CurrentCamera

workspace:GetPropertyChangedSignal("CurrentCamera"):Connect(function()
	Camera = workspace.CurrentCamera
end)

local PASSWORD = "Premium_6339466"
local FREE_TIME = 24 * 60 * 60

local DEVICE_MODE =
	UIS.TouchEnabled
	and "PHONE"
	or "PC"

--------------------------------------------------------------
-- CHARACTER
--------------------------------------------------------------

local function Char()

	return LP.Character
		or LP.CharacterAdded:Wait()

end

local function Hum()

	return Char():
		WaitForChild(
			"Humanoid"
		)

end

local function Root()

	return Char():
		WaitForChild(
			"HumanoidRootPart"
		)

end

--------------------------------------------------------------
-- FREE ACCESS
--------------------------------------------------------------

local function FreeValid()

	local expires =
		LP:GetAttribute(
			"SuperUniFreeExpires"
		)

	return typeof(expires) == "number"
		and os.time() < expires

end

local function ActivateFree()

	if LP:GetAttribute(
		"SuperUniFreeUsed"
	) then
		return false
	end

	LP:SetAttribute(
		"SuperUniFreeUsed",
		true
	)

	LP:SetAttribute(
		"SuperUniFreeExpires",
		os.time() + FREE_TIME
	)

	return true

end

--------------------------------------------------------------
-- REMOVE OLD
--------------------------------------------------------------

for _,name in ipairs({

	"SuperUniV11",
	"SuperUniAccessV11",

	"SuperUniV10",
	"SuperUniAccessV10",

	"SuperUniV9",
	"SuperUniAccessV9"

}) do

	local old =
		PG:FindFirstChild(
			name
		)

	if old then
		old:Destroy()
	end

end

pcall(function()

	RunService:
		UnbindFromRenderStep(
			"SuperUniV11_Aim"
		)

end)

--==============================================================
-- START SUPERUNI
--==============================================================

local function StartSuperUni(
	deviceMode
)

	local alive =
		true

	local isPC =
		deviceMode == "PC"

	local isPhone =
		deviceMode == "PHONE"

	----------------------------------------------------------
	-- COLORS
	----------------------------------------------------------

	local NEON_CYAN =
		Color3.fromRGB(
			0,255,255
		)

	local NEON_MAGENTA =
		Color3.fromRGB(
			255,0,210
		)

	local NEON_BLUE =
		Color3.fromRGB(
			0,120,255
		)

	local FRIEND_GREEN =
		Color3.fromRGB(
			0,255,110
		)

	local PHONE_BG =
		Color3.fromRGB(
			4,10,18
		)

	local PHONE_PANEL =
		Color3.fromRGB(
			7,18,29
		)

	local PHONE_BUTTON =
		Color3.fromRGB(
			8,27,40
		)

	local PHONE_BUTTON_ON =
		Color3.fromRGB(
			8,52,62
		)

	----------------------------------------------------------
	-- STATES
	----------------------------------------------------------

	local S = {

		infiniteJump = false,

		god = false,

		platformStand = false,

		fly = false,

		flyUp = false,

		flyDown = false,

		noclip = false,

		hover = false,

		spin = false,

		esp = false,

		names = false,

		distance = false,

		cameraLock = false,

		aimEnabled = false,

		aimTarget = nil,

		aimFOV =
			isPC
			and 500
			or 280,

		aimMaxDistance =
			600,

		coords = false,

		fps = false,

		ping = false,

		clock = false,

		playerCount = false,

		job = false,

		health = false,

		speedHUD = false,

		gravityHUD = false,

		fovHUD = false
	}

	----------------------------------------------------------
	-- FRIENDS
	----------------------------------------------------------

	local Friends = {}

	local function IsFriend(
		player
	)

		if not player then
			return false
		end

		return Friends[
			player.UserId
		] == true

	end

	local function FriendCount()

		local count =
			0

		for _ in pairs(
			Friends
		) do

			count += 1

		end

		return count

	end

	----------------------------------------------------------
	-- HOTKEYS
	----------------------------------------------------------

	local Hotkeys = {

		Menu =
			Enum.KeyCode.Tab,

		Fly =
			Enum.KeyCode.F,

		FlyUp =
			Enum.KeyCode.Eight,

		FlyDown =
			Enum.KeyCode.Two,

		Noclip =
			Enum.KeyCode.N,

		Aim =
			Enum.KeyCode.Q,

		ESP =
			Enum.KeyCode.E,

		Speed =
			Enum.KeyCode.V,

		Heal =
			Enum.KeyCode.H
	}

	----------------------------------------------------------
	-- VARIABLES
	----------------------------------------------------------

	local collisionCache =
		{}

	local forceField
	local crosshair
	local selfHighlight
	local decoy

	local cameraTarget

	local teleportPoints =
		{}

	local pointCounter =
		0

	local homeCFrame
	local previousCFrame

	local spectateIndex =
		0

	local normalMaxHealth =
		Hum().MaxHealth

	----------------------------------------------------------
	-- ORIGINAL VALUES
	----------------------------------------------------------

	local originalGravity =
		workspace.Gravity

	local originalBrightness =
		Lighting.Brightness

	local originalClock =
		Lighting.ClockTime

	local originalFogStart =
		Lighting.FogStart

	local originalFogEnd =
		Lighting.FogEnd

	local originalAmbient =
		Lighting.Ambient

	local originalOutdoor =
		Lighting.OutdoorAmbient

	----------------------------------------------------------
	-- MENU SIZE
	----------------------------------------------------------

	local viewport =
		Camera.ViewportSize

	local MENU_W
	local MENU_H

	if isPC then

		MENU_W =
			math.min(
				800,
				viewport.X - 30
			)

		MENU_H =
			math.min(
				720,
				viewport.Y - 30
			)

	else

		MENU_W =
			math.min(
				320,
				viewport.X - 12
			)

		MENU_H =
			math.min(
				430,
				viewport.Y - 18
			)

	end

	local SCALE =
		isPC
		and 1.35
		or 1

	local function PX(n)

		return math.floor(
			n * SCALE + 0.5
		)

	end

	----------------------------------------------------------
	-- GUI
	----------------------------------------------------------

	local GUI =
		Instance.new(
			"ScreenGui"
		)

	GUI.Name =
		"SuperUniV11"

	GUI.ResetOnSpawn =
		false

	GUI.IgnoreGuiInset =
		true

	GUI.DisplayOrder =
		999999

	GUI.ZIndexBehavior =
		Enum.ZIndexBehavior.Sibling

	GUI.Parent =
		PG

	----------------------------------------------------------
	-- UI HELPERS
	----------------------------------------------------------

	local function Round(
		object,
		radius
	)

		local corner =
			Instance.new(
				"UICorner"
			)

		corner.CornerRadius =
			UDim.new(
				0,
				radius or 8
			)

		corner.Parent =
			object

		return corner

	end

	local function Stroke(
		object,
		color,
		thickness,
		transparency
	)

		local stroke =
			Instance.new(
				"UIStroke"
			)

		stroke.Color =
			color
			or
			Color3.fromRGB(
				85,150,205
			)

		stroke.Thickness =
			thickness
			or
			1.5

		stroke.Transparency =
			transparency
			or
			0.35

		stroke.Parent =
			object

		return stroke

	end

	local function NeonGradient(
		object,
		rotation
	)

		local gradient =
			Instance.new(
				"UIGradient"
			)

		gradient.Color =
			ColorSequence.new({

				ColorSequenceKeypoint.new(
					0,
					NEON_CYAN
				),

				ColorSequenceKeypoint.new(
					0.5,
					NEON_BLUE
				),

				ColorSequenceKeypoint.new(
					1,
					NEON_MAGENTA
				)

			})

		gradient.Rotation =
			rotation
			or 0

		gradient.Parent =
			object

		return gradient

	end

	----------------------------------------------------------
	-- FLOAT ICON
	----------------------------------------------------------

	local Icon =
		Instance.new(
			"TextButton"
		)

	Icon.Size =
		UDim2.fromOffset(
			54,
			54
		)

	Icon.Position =
		UDim2.fromOffset(
			10,
			150
		)

	Icon.BorderSizePixel =
		0

	Icon.Text =
		"⚡"

	Icon.TextSize =
		28

	Icon.Font =
		Enum.Font.GothamBold

	Icon.TextColor3 =
		Color3.new(
			1,1,1
		)

	Icon.ZIndex =
		300

	Icon.Parent =
		GUI

	Round(
		Icon,
		27
	)

	if isPhone then

		Icon.BackgroundColor3 =
			Color3.fromRGB(
				2,13,23
			)

		local iconStroke =
			Stroke(
				Icon,
				NEON_CYAN,
				2.5,
				0.05
			)

		NeonGradient(
			Icon,
			45
		)

		TweenService:
			Create(

				iconStroke,

				TweenInfo.new(
					1.2,
					Enum.EasingStyle.Sine,
					Enum.EasingDirection.InOut,
					-1,
					true
				),

				{
					Color =
						NEON_MAGENTA,

					Transparency =
						0.3
				}

			):
			Play()

	else

		Icon.BackgroundColor3 =
			Color3.fromRGB(
				28,73,114
			)

		Stroke(
			Icon,
			nil,
			1.5,
			0.35
		)

	end

	----------------------------------------------------------
	-- MAIN
	----------------------------------------------------------

	local Main =
		Instance.new(
			"Frame"
		)

	Main.Size =
		UDim2.fromOffset(
			MENU_W,
			MENU_H
		)

	Main.Position =
		UDim2.fromOffset(

			math.max(
				5,
				(
					viewport.X
					-
					MENU_W
				)/2
			),

			math.max(
				5,
				(
					viewport.Y
					-
					MENU_H
				)/2
			)

		)

	Main.BorderSizePixel =
		0

	Main.ZIndex =
		50

	Main.Parent =
		GUI

	Round(
		Main,
		isPhone
		and 18
		or 15
	)

	if isPhone then

		Main.BackgroundColor3 =
			PHONE_BG

		Main.BackgroundTransparency =
			0.03

		local mainStroke =
			Stroke(
				Main,
				NEON_CYAN,
				2.2,
				0.08
			)

		TweenService:
			Create(

				mainStroke,

				TweenInfo.new(
					1.5,
					Enum.EasingStyle.Sine,
					Enum.EasingDirection.InOut,
					-1,
					true
				),

				{
					Color =
						NEON_MAGENTA
				}

			):
			Play()

	else

		Main.BackgroundColor3 =
			Color3.fromRGB(
				21,47,72
			)

		Main.BackgroundTransparency =
			0.04

		Stroke(
			Main,
			nil,
			2,
			0.35
		)

	end

	----------------------------------------------------------
	-- TITLE
	----------------------------------------------------------

	local Title =
		Instance.new(
			"TextLabel"
		)

	Title.Size =
		UDim2.new(
			1,-PX(55),
			0,PX(44)
		)

	Title.Position =
		UDim2.fromOffset(
			PX(12),
			0
		)

	Title.BackgroundTransparency =
		1

	Title.Text =
		isPhone
		and
			"⚡ SUPERUNI NEON V11"
		or
			"⚡ SuperUni V11 • 120"

	Title.TextColor3 =
		Color3.new(
			1,1,1
		)

	Title.TextSize =
		isPhone
		and 17
		or PX(17)

	Title.Font =
		Enum.Font.GothamBold

	Title.TextXAlignment =
		Enum.TextXAlignment.Left

	Title.Active =
		true

	Title.ZIndex =
		60

	Title.Parent =
		Main

	if isPhone then

		NeonGradient(
			Title,
			0
		)

	end

	----------------------------------------------------------
	-- CLOSE
	----------------------------------------------------------

	local Close =
		Instance.new(
			"TextButton"
		)

	Close.Size =
		UDim2.fromOffset(
			PX(34),
			PX(30)
		)

	Close.Position =
		UDim2.new(
			1,-PX(40),
			0,PX(7)
		)

	Close.BorderSizePixel =
		0

	Close.Text =
		"×"

	Close.TextSize =
		PX(21)

	Close.TextColor3 =
		Color3.new(
			1,1,1
		)

	Close.ZIndex =
		61

	Close.Parent =
		Main

	Round(
		Close,
		8
	)

	if isPhone then

		Close.BackgroundColor3 =
			Color3.fromRGB(
				37,6,37
			)

		Stroke(
			Close,
			NEON_MAGENTA,
			1.6,
			0.05
		)

	else

		Close.BackgroundColor3 =
			Color3.fromRGB(
				35,72,105
			)

	end

	----------------------------------------------------------
	-- CLAMP
	----------------------------------------------------------

	local function ClampObject(
		object,
		x,
		y
	)

		local v =
			Camera.ViewportSize

		local size =
			object.AbsoluteSize

		x =
			math.clamp(
				x,
				5,
				math.max(
					5,
					v.X-size.X-5
				)
			)

		y =
			math.clamp(
				y,
				5,
				math.max(
					5,
					v.Y-size.Y-5
				)
			)

		object.Position =
			UDim2.fromOffset(
				x,
				y
			)

	end

	----------------------------------------------------------
	-- DRAG MAIN
	----------------------------------------------------------

	local dragging =
		false

	local dragStart
	local dragOriginal

	Title.InputBegan:
	Connect(function(input)

		if input.UserInputType
			==
			Enum.UserInputType.MouseButton1

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			dragging =
				true

			dragStart =
				input.Position

			dragOriginal =
				Main.AbsolutePosition

		end

	end)

	UIS.InputChanged:
	Connect(function(input)

		if not dragging then
			return
		end

		if input.UserInputType
			==
			Enum.UserInputType.MouseMovement

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			local delta =
				input.Position
				-
				dragStart

			ClampObject(
				Main,

				dragOriginal.X
				+
				delta.X,

				dragOriginal.Y
				+
				delta.Y
			)

		end

	end)

	UIS.InputEnded:
	Connect(function(input)

		if input.UserInputType
			==
			Enum.UserInputType.MouseButton1

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			dragging =
				false

		end

	end)

	----------------------------------------------------------
	-- ICON DRAG
	----------------------------------------------------------

	local iconDragging =
		false

	local iconMoved =
		false

	local iconStart
	local iconOriginal

	Icon.InputBegan:
	Connect(function(input)

		if input.UserInputType
			==
			Enum.UserInputType.MouseButton1

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			iconDragging =
				true

			iconMoved =
				false

			iconStart =
				input.Position

			iconOriginal =
				Icon.AbsolutePosition

		end

	end)

	UIS.InputChanged:
	Connect(function(input)

		if not iconDragging then
			return
		end

		if input.UserInputType
			==
			Enum.UserInputType.MouseMovement

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			local delta =
				input.Position
				-
				iconStart

			if delta.Magnitude
				>
				7
			then

				iconMoved =
					true

			end

			ClampObject(
				Icon,

				iconOriginal.X
				+
				delta.X,

				iconOriginal.Y
				+
				delta.Y
			)

		end

	end)

	UIS.InputEnded:
	Connect(function(input)

		if input.UserInputType
			==
			Enum.UserInputType.MouseButton1

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			iconDragging =
				false

		end

	end)

	Icon.Activated:
	Connect(function()

		if not iconMoved then

			Main.Visible =
				not Main.Visible

		end

	end)

	Close.Activated:
	Connect(function()

		Main.Visible =
			false

	end)

	----------------------------------------------------------
	-- PC
	----------------------------------------------------------

	if isPC then

		Icon.Visible =
			false

		pcall(function()

			StarterGui:
				SetCoreGuiEnabled(
					Enum.CoreGuiType.PlayerList,
					false
				)

		end)

	end

	----------------------------------------------------------
	-- TABS
	----------------------------------------------------------

	local TabBar =
		Instance.new(
			"ScrollingFrame"
		)

	TabBar.Size =
		UDim2.new(
			1,-PX(12),
			0,PX(43)
		)

	TabBar.Position =
		UDim2.fromOffset(
			PX(6),
			PX(47)
		)

	TabBar.BackgroundTransparency =
		1

	TabBar.BorderSizePixel =
		0

	TabBar.ScrollBarThickness =
		0

	TabBar.ScrollingDirection =
			Enum.ScrollingDirection.X

	TabBar.CanvasSize =
		UDim2.new()

	TabBar.ZIndex =
		60

	TabBar.Parent =
		Main

	local tabLayout =
		Instance.new(
			"UIListLayout"
		)

	tabLayout.FillDirection =
		Enum.FillDirection.Horizontal

	tabLayout.Padding =
		UDim.new(
			0,
			PX(5)
		)

	tabLayout.Parent =
		TabBar

	tabLayout:
		GetPropertyChangedSignal(
			"AbsoluteContentSize"
		):
		Connect(function()

			TabBar.CanvasSize =
				UDim2.fromOffset(

					tabLayout.AbsoluteContentSize.X
					+
					PX(10),

					0
				)

		end)

	----------------------------------------------------------
	-- PAGE HOLDER
	----------------------------------------------------------

	local PageHolder =
		Instance.new(
			"Frame"
		)

	PageHolder.Size =
		UDim2.new(
			1,-PX(12),
			1,-PX(98)
		)

	PageHolder.Position =
		UDim2.fromOffset(
			PX(6),
			PX(94)
		)

	PageHolder.BackgroundTransparency =
		1

	PageHolder.Active =
		true

	PageHolder.ZIndex =
		55

	PageHolder.Parent =
		Main

	local pages =
		{}

	local tabs =
		{}

	local categoryOrder = {

		"PLAYER",
		"MOVE",
		"VISUAL",
		"TP",
		"TOOLS",
		"HOTKEYS"

	}

	local currentPageIndex =
		1

	----------------------------------------------------------
	-- CREATE PAGE
	----------------------------------------------------------

	local function CreatePage(
		name
	)

		local page =
			Instance.new(
				"ScrollingFrame"
			)

		page.Name =
			name

		page.Size =
			UDim2.fromScale(
				1,1
			)

		page.BackgroundTransparency =
			1

		page.BorderSizePixel =
			0

		page.ScrollBarThickness =
			PX(4)

		page.ScrollBarImageColor3 =
			isPhone
			and
				NEON_CYAN
			or
				Color3.fromRGB(
					90,160,220
				)

		page.ScrollingDirection =
			Enum.ScrollingDirection.Y

		page.ElasticBehavior =
			Enum.ElasticBehavior.Always

		page.CanvasSize =
			UDim2.new()

		page.Active =
			true

		page.Visible =
			false

		page.Parent =
			PageHolder

		local padding =
			Instance.new(
				"UIPadding"
			)

		padding.PaddingTop =
			UDim.new(
				0,
				PX(4)
			)

		padding.PaddingBottom =
			UDim.new(
				0,
				PX(70)
			)

		padding.Parent =
			page

		local layout =
			Instance.new(
				"UIListLayout"
			)

		layout.Padding =
			UDim.new(
				0,
				PX(6)
			)

		layout.HorizontalAlignment =
			Enum.HorizontalAlignment.Center

		layout.SortOrder =
			Enum.SortOrder.LayoutOrder

		layout.Parent =
			page

		local function UpdateCanvas()

			page.CanvasSize =
				UDim2.new(
					0,0,
					0,

					layout.AbsoluteContentSize.Y
					+
					PX(85)
				)

		end

		layout:
			GetPropertyChangedSignal(
				"AbsoluteContentSize"
			):
			Connect(
				UpdateCanvas
			)

		task.defer(
			UpdateCanvas
		)

		pages[name] =
			page

		return page

	end

	local PlayerPage =
		CreatePage(
			"PLAYER"
		)

	local MovePage =
		CreatePage(
			"MOVE"
		)

	local VisualPage =
		CreatePage(
			"VISUAL"
		)

	local TPPage =
		CreatePage(
			"TP"
		)

	local ToolsPage =
		CreatePage(
			"TOOLS"
		)

	local HotkeyPage =
		CreatePage(
			"HOTKEYS"
		)

	----------------------------------------------------------
	-- SHOW PAGE
	----------------------------------------------------------

	local function ShowPage(
		name
	)

		for i,pageName in ipairs(
			categoryOrder
		) do

			if pageName
				==
				name
			then

				currentPageIndex =
					i

				break

			end

		end

		for pageName,page in pairs(
			pages
		) do

			page.Visible =
				pageName
				==
				name

		end

		for tabName,tab in pairs(
			tabs
		) do

			local selected =
				tabName
				==
				name

			if isPhone then

				tab.BackgroundColor3 =
					selected
					and
					Color3.fromRGB(
						8,42,55
					)
					or
					Color3.fromRGB(
						6,19,30
					)

				local st =
					tab:
						FindFirstChildOfClass(
							"UIStroke"
						)

				if st then

					st.Color =
						selected
							and
							NEON_MAGENTA
							or
							NEON_CYAN

				end

			else

				tab.BackgroundColor3 =
					selected
						and
						Color3.fromRGB(
							42,105,155
						)
						or
						Color3.fromRGB(
							31,67,99
						)

			end

		end

	end

	----------------------------------------------------------
	-- CREATE TAB
	----------------------------------------------------------

	local function CreateTab(
		name,
		text
	)

		local b =
			Instance.new(
				"TextButton"
			)

		b.Size =
			UDim2.fromOffset(

				isPC
				and 125
				or 82,

				PX(37)
			)

		b.BorderSizePixel =
			0

		b.Text =
			text

		b.TextColor3 =
			Color3.new(
				1,1,1
			)

		b.TextSize =
			isPC
			and 15
			or 10

		b.Font =
			Enum.Font.GothamBold

		b.Parent =
			TabBar

		Round(
			b,
			8
		)

		if isPhone then

			b.BackgroundColor3 =
				Color3.fromRGB(
					6,19,30
				)

			Stroke(
				b,
				NEON_CYAN,
				1.3,
				0.3
			)

		else

			b.BackgroundColor3 =
				Color3.fromRGB(
					31,67,99
				)

		end

		tabs[name] =
			b

		b.Activated:
		Connect(function()

			ShowPage(
				name
			)

		end)

	end

	CreateTab(
		"PLAYER",
		"👤 PLAYER"
	)

	CreateTab(
		"MOVE",
		"✈ MOVE"
	)

	CreateTab(
		"VISUAL",
		"👁 VISUAL"
	)

	CreateTab(
		"TP",
		"📍 TP"
	)

	CreateTab(
		"TOOLS",
		"⚙ TOOLS"
	)

	CreateTab(
		"HOTKEYS",
		"⌨ KEYS"
	)

	ShowPage(
		"PLAYER"
	)

	--==========================================================
	-- SWIPE CATEGORIES
	--==========================================================

	local swipeActive =
		false

	local swipeStart
	local swipeLast

	local SWIPE_DISTANCE =
		isPC
		and 100
		or 65

	local function Inside(
		object,
		pos
	)

		local p =
			object.AbsolutePosition

		local s =
			object.AbsoluteSize

		return pos.X >= p.X
			and pos.X <= p.X+s.X
			and pos.Y >= p.Y
			and pos.Y <= p.Y+s.Y

	end

	UIS.InputBegan:
	Connect(function(input)

		if not alive
			or not Main.Visible
		then
			return
		end

		if input.UserInputType
			~=
			Enum.UserInputType.MouseButton1

			and

			input.UserInputType
			~=
			Enum.UserInputType.Touch
		then
			return
		end

		if not Inside(
			PageHolder,
			input.Position
		) then
			return
		end

		swipeActive =
			true

		swipeStart =
			input.Position

		swipeLast =
			input.Position

	end)

	UIS.InputChanged:
	Connect(function(input)

		if not swipeActive then
			return
		end

		if input.UserInputType
			==
			Enum.UserInputType.MouseMovement

			or

			input.UserInputType
			==
			Enum.UserInputType.Touch
		then

			swipeLast =
				input.Position

		end

	end)

	UIS.InputEnded:
	Connect(function(input)

		if not swipeActive then
			return
		end

		if input.UserInputType
			~=
			Enum.UserInputType.MouseButton1

			and

			input.UserInputType
			~=
			Enum.UserInputType.Touch
		then
			return
		end

		local delta =
			(swipeLast or input.Position)
			-
			swipeStart

		swipeActive =
			false

		if math.abs(
			delta.X
		)
			<
			SWIPE_DISTANCE
		then
			return
		end

		if math.abs(
			delta.X
		)
			<
			math.abs(
				delta.Y
			)
			*
			1.25
		then
			return
		end

		if delta.X < 0 then

			currentPageIndex =
				math.min(
					#categoryOrder,
					currentPageIndex + 1
				)

		else

			currentPageIndex =
				math.max(
					1,
					currentPageIndex - 1
				)

		end

		ShowPage(
			categoryOrder[
				currentPageIndex
			]
		)

	end)

	----------------------------------------------------------
	-- BUTTON
	----------------------------------------------------------

	local function Button(
		page,
		number,
		text,
		callback
	)

		local b =
			Instance.new(
				"TextButton"
			)

		b.Size =
			UDim2.new(
				1,-PX(8),
				0,PX(43)
			)

		b.BorderSizePixel =
			0

		if number then

			b.Text =
				number
				..
				". "
				..
				text

			b.LayoutOrder =
				number

		else

			b.Text =
				text

		end

		b.TextColor3 =
			Color3.fromRGB(
				240,248,255
			)

		b.TextSize =
			isPC
			and 16
			or 11

		b.Font =
			Enum.Font.GothamSemibold

		b.Parent =
			page

		Round(
			b,
			isPhone
			and 10
			or 8
		)

		if isPhone then

			b.BackgroundColor3 =
				PHONE_BUTTON

			Stroke(
				b,
				NEON_CYAN,
				1.2,
				0.45
			)

		else

			b.BackgroundColor3 =
				Color3.fromRGB(
					35,75,110
				)

		end

		b.Activated:
		Connect(function()

			if alive then

				callback(
					b
				)

			end

		end)

		return b

	end

	----------------------------------------------------------
	-- TOGGLE
	----------------------------------------------------------

	local function ToggleStyle(
		button,
		enabled,
		label
	)

		button.Text =
			label
			..
			(
				enabled
					and ": ON"
					or ": OFF"
			)

		if isPhone then

			button.BackgroundColor3 =
				enabled
					and
					PHONE_BUTTON_ON
					or
					PHONE_BUTTON

			local st =
				button:
					FindFirstChildOfClass(
						"UIStroke"
					)

			if st then

				st.Color =
					enabled
						and
						NEON_MAGENTA
						or
						NEON_CYAN

				st.Transparency =
					enabled
						and 0
						or 0.45

			end

		else

			button.BackgroundColor3 =
				enabled
					and
					Color3.fromRGB(
						35,110,160
					)
					or
					Color3.fromRGB(
						35,75,110
					)

		end

	end

	----------------------------------------------------------
	-- TELEPORT
	----------------------------------------------------------

	local function TeleportTo(
		cf
	)

		if not cf then
			return
		end

		previousCFrame =
			Root().CFrame

		Root().CFrame =
			cf

	end

	----------------------------------------------------------
	-- FIND PLAYER
	----------------------------------------------------------

	local function NearestPlayer(
		ignoreFriends
	)

		local root =
			Root()

		local best
		local bestDistance =
			math.huge

		for _,p in ipairs(
			Players:GetPlayers()
		) do

			if p ~= LP
				and p.Character
				and
				(
					not ignoreFriends
					or
					not IsFriend(p)
				)
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				local h =
					p.Character:
						FindFirstChildOfClass(
							"Humanoid"
						)

				if r
					and h
					and h.Health > 0
				then

					local d =
						(
							r.Position
							-
							root.Position
						).Magnitude

					if d
						<
						bestDistance
					then

						best =
							p

						bestDistance =
							d

					end

				end

			end

		end

		return best

	end

	local function RandomPlayer()

		local list =
			{}

		for _,p in ipairs(
			Players:GetPlayers()
		) do

			if p ~= LP
				and p.Character
			then

				table.insert(
					list,
					p
				)

			end

		end

		if #list == 0 then
			return nil
		end

		return list[
			math.random(
				1,
				#list
			)
		]

	end

	--==========================================================
	-- FRIEND SYSTEM
	--==========================================================

	local function PlayerFromPart(
		part
	)

		if not part then
			return nil
		end

		local current =
			part

		while current
			and current ~= workspace
		do

			if current:IsA(
				"Model"
			) then

				local player =
					Players:
						GetPlayerFromCharacter(
							current
						)

				if player then
					return player
				end

			end

			current =
				current.Parent

		end

		return nil

	end

	local function Notification(
		title,
		text
	)

		pcall(function()

			StarterGui:
				SetCore(
					"SendNotification",
					{
						Title =
							title,

						Text =
							text,

						Duration =
							2.5
					}
				)

		end)

	end

	----------------------------------------------------------
	-- DECOY
	----------------------------------------------------------

	local function RemoveDecoy()

		if decoy then

			decoy:
				Destroy()

			decoy =
				nil

		end

	end

	local function CreateDecoy()

		RemoveDecoy()

		local character =
			Char()

		local old =
			character.Archivable

		character.Archivable =
			true

		decoy =
			character:
				Clone()

		character.Archivable =
			old

		decoy.Name =
			"SuperUni_Decoy"

		for _,obj in ipairs(
			decoy:
				GetDescendants()
		) do

			if obj:IsA(
				"Script"
			)
				or
				obj:IsA(
					"LocalScript"
				)
			then

				obj:
					Destroy()

			elseif obj:IsA(
				"BasePart"
			) then

				obj.Anchored =
					true

				obj.CanCollide =
					false

			end

		end

		decoy.Parent =
			workspace

	end

	--==========================================================
	-- PLAYER 1-24
	--==========================================================

	Button(
		PlayerPage,
		1,
		"⚡ Speed 45",
		function()

			Hum().WalkSpeed =
				45

		end
	)

	Button(
		PlayerPage,
		2,
		"🚀 Speed 80",
		function()

			Hum().WalkSpeed =
				80

		end
	)

	Button(
		PlayerPage,
		3,
		"🐌 Speed 8",
		function()

			Hum().WalkSpeed =
				8

		end
	)

	Button(
		PlayerPage,
		4,
		"🚶 Speed 16",
		function()

			Hum().WalkSpeed =
				16

		end
	)

	Button(
		PlayerPage,
		5,
		"⚡ Speed 25",
		function()

			Hum().WalkSpeed =
				25

		end
	)

	Button(
		PlayerPage,
		6,
		"🔥 Speed 60",
		function()

			Hum().WalkSpeed =
				60

		end
	)

	Button(
		PlayerPage,
		7,
		"🦘 Jump 100",
		function()

			Hum().UseJumpPower =
				true

			Hum().JumpPower =
				100

		end
	)

	Button(
		PlayerPage,
		8,
		"🐇 Jump 25",
		function()

			Hum().UseJumpPower =
				true

			Hum().JumpPower =
				25

		end
	)

	Button(
		PlayerPage,
		9,
		"🦘 Jump 50",
		function()

			Hum().UseJumpPower =
				true

			Hum().JumpPower =
				50

		end
	)

	Button(
		PlayerPage,
		10,
		"♾ Infinite Jump",
		function(b)

			S.infiniteJump =
				not
				S.infiniteJump

			ToggleStyle(
				b,
				S.infiniteJump,
				"10. ♾ INFINITE JUMP"
			)

		end
	)

	Button(
		PlayerPage,
		11,
		"❤️ Heal",
		function()

			Hum().Health =
				Hum().MaxHealth

		end
	)

	Button(
		PlayerPage,
		12,
		"🛡 God + Decoy",
		function(b)

			S.god =
				not S.god

			if S.god then

				normalMaxHealth =
					Hum().MaxHealth

				CreateDecoy()

				Hum().MaxHealth =
					1000000000

				Hum().Health =
					Hum().MaxHealth

			else

				Hum().MaxHealth =
					normalMaxHealth

				Hum().Health =
					math.min(
						Hum().Health,
						normalMaxHealth
					)

				RemoveDecoy()

			end

			ToggleStyle(
				b,
				S.god,
				"12. 🛡 GOD + DECOY"
			)

		end
	)

	Button(
		PlayerPage,
		13,
		"🛡 ForceField",
		function(b)

			if forceField then

				forceField:
					Destroy()

				forceField =
					nil

				ToggleStyle(
					b,
					false,
					"13. 🛡 FORCEFIELD"
				)

			else

				forceField =
					Instance.new(
						"ForceField"
					)

				forceField.Parent =
					Char()

				ToggleStyle(
					b,
					true,
					"13. 🛡 FORCEFIELD"
				)

			end

		end
	)

	Button(
		PlayerPage,
		14,
		"👻 Invisible",
		function()

			for _,x in ipairs(
				Char():
					GetDescendants()
			) do

				if x:IsA(
					"BasePart"
				) then

					x.LocalTransparencyModifier =
						1

				end

			end

		end
	)

	Button(
		PlayerPage,
		15,
		"👤 Visible",
		function()

			for _,x in ipairs(
				Char():
					GetDescendants()
			) do

				if x:IsA(
					"BasePart"
				) then

					x.LocalTransparencyModifier =
						0

				end

			end

		end
	)

	Button(
		PlayerPage,
		16,
		"🪑 Sit",
		function()

			Hum().Sit =
				true

		end
	)

	Button(
		PlayerPage,
		17,
		"🚶 Stand",
		function()

			Hum().Sit =
				false

		end
	)

	Button(
		PlayerPage,
		18,
		"❄ Freeze",
		function()

			Root().Anchored =
				true

		end
	)

	Button(
		PlayerPage,
		19,
		"🔥 Unfreeze",
		function()

			Root().Anchored =
				false

		end
	)

	Button(
		PlayerPage,
		20,
		"☠ Reset Character",
		function()

			Hum().Health =
				0

		end
	)

	Button(
		PlayerPage,
		21,
		"⬆ HipHeight +1",
		function()

			Hum().HipHeight +=
				1

		end
	)

	Button(
		PlayerPage,
		22,
		"⬇ HipHeight -1",
		function()

			Hum().HipHeight -=
				1

		end
	)

	Button(
		PlayerPage,
		23,
		"♻ Reset HipHeight",
		function()

			Hum().HipHeight =
				0

		end
	)

	Button(
		PlayerPage,
		24,
		"🧍 PlatformStand",
		function(b)

			S.platformStand =
				not
				S.platformStand

			Hum().PlatformStand =
				S.platformStand

			ToggleStyle(
				b,
				S.platformStand,
				"24. 🧍 PLATFORM"
			)

		end
	)

	UIS.JumpRequest:
	Connect(function()

		if alive
			and
			S.infiniteJump
		then

			Hum():
				ChangeState(
					Enum.HumanoidStateType.Jumping
				)

		end

	end)

	--==========================================================
	-- MOVE 25-48
	--==========================================================

	local FlyButton

	FlyButton =
		Button(
			MovePage,
			25,
			"✈ Fly",
			function(b)

				S.fly =
					not S.fly

				if not S.fly then

					S.flyUp =
						false

					S.flyDown =
						false

				end

				ToggleStyle(
					b,
					S.fly,
					"25. ✈ FLY"
				)

			end
		)

	Button(
		MovePage,
		26,
		"⬆ Fly Up",
		function(b)

			S.flyUp =
				not
				S.flyUp

			if S.flyUp then

				S.flyDown =
					false

			end

			ToggleStyle(
				b,
				S.flyUp,
				"26. ⬆ FLY UP"
			)

		end
	)

	Button(
		MovePage,
		27,
		"⬇ Fly Down",
		function(b)

			S.flyDown =
				not
				S.flyDown

			if S.flyDown then

				S.flyUp =
					false

			end

			ToggleStyle(
				b,
				S.flyDown,
				"27. ⬇ FLY DOWN"
			)

		end
	)

	local NoclipButton

	NoclipButton =
		Button(
			MovePage,
			28,
			"👻 Noclip",
			function(b)

				S.noclip =
					not
					S.noclip

				ToggleStyle(
					b,
					S.noclip,
					"28. 👻 NOCLIP"
				)

			end
		)

	Button(
		MovePage,
		29,
		"☁ Hover",
		function(b)

			S.hover =
				not
				S.hover

			ToggleStyle(
				b,
				S.hover,
				"29. ☁ HOVER"
			)

		end
	)

	Button(
		MovePage,
		30,
		"🌀 Spin",
		function(b)

			S.spin =
				not
				S.spin

			ToggleStyle(
				b,
				S.spin,
				"30. 🌀 SPIN"
			)

		end
	)

	Button(
		MovePage,
		31,
		"⛔ Stop Spin",
		function()

			S.spin =
				false

			Root().AssemblyAngularVelocity =
				Vector3.zero

		end
	)

	Button(
		MovePage,
		32,
		"🌌 Gravity 0",
		function()

			workspace.Gravity =
				0

		end
	)

	Button(
		MovePage,
		33,
		"🌙 Gravity 60",
		function()

			workspace.Gravity =
				60

		end
	)

	Button(
		MovePage,
		34,
		"🌍 Normal Gravity",
		function()

			workspace.Gravity =
				originalGravity

		end
	)

	Button(
		MovePage,
		35,
		"💨 Dash Forward",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					0,0,-20
				)
			)

		end
	)

	Button(
		MovePage,
		36,
		"💨 Dash Back",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					0,0,20
				)
			)

		end
	)

	Button(
		MovePage,
		37,
		"⬅ Dash Left",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					-20,0,0
				)
			)

		end
	)

	Button(
		MovePage,
		38,
		"➡ Dash Right",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					20,0,0
				)
			)

		end
	)

	Button(
		MovePage,
		39,
		"⬆ Move Up 10",
		function()

			TeleportTo(
				Root().CFrame
				+
				Vector3.new(
					0,10,0
				)
			)

		end
	)

	Button(
		MovePage,
		40,
		"⬇ Move Down 10",
		function()

			TeleportTo(
				Root().CFrame
				+
				Vector3.new(
					0,-10,0
				)
			)

		end
	)

	Button(
		MovePage,
		41,
		"⏩ Forward 50",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					0,0,-50
				)
			)

		end
	)

	Button(
		MovePage,
		42,
		"⏪ Back 50",
		function()

			TeleportTo(
				Root().CFrame
				*
				CFrame.new(
					0,0,50
				)
			)

		end
	)

	Button(
		MovePage,
		43,
		"🔄 Turn Around",
		function()

			Root().CFrame =
				Root().CFrame
				*
				CFrame.Angles(
					0,
					math.rad(
						180
					),
					0
				)

		end
	)

	Button(
		MovePage,
		44,
		"🦘 Jump Now",
		function()

			Hum():
				ChangeState(
					Enum.HumanoidStateType.Jumping
				)

		end
	)

	Button(
		MovePage,
		45,
		"🧭 AutoRotate",
		function(b)

			Hum().AutoRotate =
				not
					Hum().AutoRotate

			ToggleStyle(
				b,
				Hum().AutoRotate,
				"45. 🧭 AUTOROTATE"
			)

		end
	)

	Button(
		MovePage,
		46,
		"➕ Speed +5",
		function()

			Hum().WalkSpeed +=
				5

		end
	)

	Button(
		MovePage,
		47,
		"➖ Speed -5",
		function()

			Hum().WalkSpeed =
				math.max(
					0,
					Hum().WalkSpeed - 5
				)

		end
	)

	Button(
		MovePage,
		48,
		"♻ Reset Movement",
		function()

			S.fly =
				false

			S.flyUp =
				false

			S.flyDown =
				false

			S.hover =
				false

			S.spin =
				false

			Hum().WalkSpeed =
				16

			Hum().UseJumpPower =
				true

			Hum().JumpPower =
				50

			Hum().AutoRotate =
				true

			Root().Anchored =
				false

			Root().AssemblyAngularVelocity =
				Vector3.zero

			ToggleStyle(
				FlyButton,
				false,
				"25. ✈ FLY"
			)

		end
	)

	--==========================================================
	-- ESP
	--==========================================================

	local function ClearVisual(
		p
	)

		if not p.Character then
			return
		end

		for _,name in ipairs({

			"SU11_ESP",
			"SU11_NAME",
			"SU11_DISTANCE"

		}) do

			local obj =
				p.Character:
					FindFirstChild(
						name,
						true
					)

			if obj then

				obj:
					Destroy()

			end

		end

	end

	local function RefreshVisual()

		for _,p in ipairs(
			Players:GetPlayers()
		) do

			if p ~= LP
				and p.Character
			then

				ClearVisual(
					p
				)

				local friend =
					IsFriend(
						p
					)

				if S.esp then

					local hi =
						Instance.new(
							"Highlight"
						)

					hi.Name =
						"SU11_ESP"

					hi.DepthMode =
						Enum.HighlightDepthMode.AlwaysOnTop

					hi.FillTransparency =
						friend
						and 0.5
						or 0.7

					hi.OutlineTransparency =
						0

					if friend then

						hi.FillColor =
							FRIEND_GREEN

						hi.OutlineColor =
							FRIEND_GREEN

					elseif isPhone then

						hi.FillColor =
							NEON_CYAN

						hi.OutlineColor =
							NEON_MAGENTA

					end

					hi.Parent =
						p.Character

				end

				local head =
					p.Character:
						FindFirstChild(
							"Head"
						)

				if head
					and
					S.names
				then

					local gui =
						Instance.new(
							"BillboardGui"
						)

					gui.Name =
						"SU11_NAME"

					gui.Size =
						UDim2.fromOffset(
							170,
							27
						)

					gui.StudsOffset =
						Vector3.new(
							0,
							2.7,
							0
						)

					gui.AlwaysOnTop =
						true

					gui.Parent =
						head

					local text =
						Instance.new(
							"TextLabel"
						)

					text.Size =
						UDim2.fromScale(
							1,1
						)

					text.BackgroundTransparency =
						1

					text.Text =
						friend
						and
							"🟢 "
							..
							p.DisplayName
						or
							p.DisplayName

					text.TextColor3 =
						friend
						and
							FRIEND_GREEN
						or
							(
								isPhone
								and
								NEON_CYAN
								or
								Color3.new(
									1,1,1
								)
							)

					text.TextScaled =
						true

					text.Font =
						Enum.Font.GothamBold

					text.Parent =
						gui

				end

				if head
					and
					S.distance
				then

					local gui =
						Instance.new(
							"BillboardGui"
						)

					gui.Name =
						"SU11_DISTANCE"

					gui.Size =
						UDim2.fromOffset(
							150,
							22
						)

					gui.StudsOffset =
						Vector3.new(
							0,
							1.8,
							0
						)

					gui.AlwaysOnTop =
						true

					gui.Parent =
						head

					local text =
						Instance.new(
							"TextLabel"
						)

					text.Name =
						"DistanceText"

					text.Size =
						UDim2.fromScale(
							1,1
						)

					text.BackgroundTransparency =
						1

					text.TextColor3 =
						friend
						and
							FRIEND_GREEN
						or
							(
								isPhone
								and
								NEON_MAGENTA
								or
								Color3.new(
									1,1,1
								)
							)

					text.TextScaled =
						true

					text.Parent =
						gui

				end

			end

		end

	end

	----------------------------------------------------------
	-- TOGGLE FRIEND
	----------------------------------------------------------

	local function ToggleFriend(
		player
	)

		if not player
			or player == LP
		then
			return
		end

		if Friends[
			player.UserId
		] then

			Friends[
				player.UserId
			] = nil

			Notification(
				"SuperUni",
				player.DisplayName
				..
				" удалён из друзей"
			)

		else

			Friends[
				player.UserId
			] =
				true

			Notification(
				"SuperUni",
				player.DisplayName
				..
				" добавлен в друзья"
			)

		end

		if S.esp
			or S.names
			or S.distance
		then

			RefreshVisual()

		end

		if S.aimTarget
			and
			IsFriend(
				S.aimTarget
			)
		then

			S.aimTarget =
				nil

		end

	end

	--==========================================================
	-- VISUAL 49-72
	--==========================================================

	local ESPButton

	ESPButton =
		Button(
			VisualPage,
			49,
			"👁 ESP",
			function(b)

				S.esp =
					not S.esp

				RefreshVisual()

				ToggleStyle(
					b,
					S.esp,
					"49. 👁 ESP"
				)

			end
		)

	Button(
		VisualPage,
		50,
		"🏷 Names",
		function(b)

			S.names =
				not S.names

			RefreshVisual()

			ToggleStyle(
				b,
				S.names,
				"50. 🏷 NAMES"
			)

		end
	)

	Button(
		VisualPage,
		51,
		"📏 Distance",
		function(b)

			S.distance =
				not S.distance

			RefreshVisual()

			ToggleStyle(
				b,
				S.distance,
				"51. 📏 DISTANCE"
			)

		end
	)

	Button(
		VisualPage,
		52,
		"💡 FullBright",
		function()

			Lighting.Brightness =
				3

			Lighting.ClockTime =
				14

			Lighting.Ambient =
				Color3.fromRGB(
					180,180,180
				)

			Lighting.OutdoorAmbient =
				Color3.fromRGB(
					180,180,180
				)

		end
	)

	Button(
		VisualPage,
		53,
		"♻ Restore Lighting",
		function()

			Lighting.Brightness =
				originalBrightness

			Lighting.ClockTime =
				originalClock

			Lighting.Ambient =
				originalAmbient

			Lighting.OutdoorAmbient =
				originalOutdoor

		end
	)

	Button(
		VisualPage,
		54,
		"☀ Day",
		function()

			Lighting.ClockTime =
				14

		end
	)

	Button(
		VisualPage,
		55,
		"🌙 Night",
		function()

			Lighting.ClockTime =
				0

		end
	)

	Button(
		VisualPage,
		56,
		"🌫 No Fog",
		function()

			Lighting.FogStart =
				100000

			Lighting.FogEnd =
				100000

		end
	)

	Button(
		VisualPage,
		57,
		"🌫 Restore Fog",
		function()

			Lighting.FogStart =
				originalFogStart

			Lighting.FogEnd =
				originalFogEnd

		end
	)

	Button(
		VisualPage,
		58,
		"🎥 FOV 60",
		function()

			Camera.FieldOfView =
				60

		end
	)

	Button(
		VisualPage,
		59,
		"🎥 FOV 70",
		function()

			Camera.FieldOfView =
				70

		end
	)

	Button(
		VisualPage,
		60,
		"🎥 FOV 90",
		function()

			Camera.FieldOfView =
				90

		end
	)

	Button(
		VisualPage,
		61,
		"🎥 FOV 110",
		function()

			Camera.FieldOfView =
				110

		end
	)

	Button(
		VisualPage,
		62,
		"🎥 FOV 120",
		function()

			Camera.FieldOfView =
				120

		end
	)

	Button(
		VisualPage,
		63,
		"✚ Crosshair",
		function()

			if crosshair then
				return
			end

			crosshair =
				Instance.new(
					"TextLabel"
				)

			crosshair.Size =
				UDim2.fromOffset(
					36,36
				)

			crosshair.AnchorPoint =
				Vector2.new(
					0.5,0.5
				)

			crosshair.Position =
				UDim2.fromScale(
					0.5,0.5
				)

			crosshair.BackgroundTransparency =
				1

			crosshair.Text =
				"+"

			crosshair.TextSize =
				31

			crosshair.TextColor3 =
				isPhone
				and
				NEON_CYAN
				or
				Color3.new(
					1,1,1
				)

			crosshair.ZIndex =
				250

			crosshair.Parent =
				GUI

		end
	)

	Button(
		VisualPage,
		64,
		"❌ Remove Crosshair",
		function()

			if crosshair then

				crosshair:
					Destroy()

				crosshair =
					nil

			end

		end
	)

	Button(
		VisualPage,
		65,
		"✨ Highlight Self",
		function()

			if selfHighlight then

				selfHighlight:
					Destroy()

			end

			selfHighlight =
				Instance.new(
					"Highlight"
				)

			selfHighlight.Parent =
				Char()

		end
	)

	Button(
		VisualPage,
		66,
		"❌ Remove Highlight",
		function()

			if selfHighlight then

				selfHighlight:
					Destroy()

				selfHighlight =
					nil

			end

		end
	)

	Button(
		VisualPage,
		67,
		"👁 First Person",
		function()

			LP.CameraMode =
				Enum.CameraMode.LockFirstPerson

		end
	)

	Button(
		VisualPage,
		68,
		"🎮 Classic Camera",
		function()

			LP.CameraMode =
				Enum.CameraMode.Classic

		end
	)

	Button(
		VisualPage,
		69,
		"🔍 Close Zoom",
		function()

			LP.CameraMinZoomDistance =
				1

			LP.CameraMaxZoomDistance =
				10

		end
	)

	Button(
		VisualPage,
		70,
		"🔭 Normal Zoom",
		function()

			LP.CameraMinZoomDistance =
				0.5

			LP.CameraMaxZoomDistance =
				128

		end
	)

	Button(
		VisualPage,
		71,
		"🙈 Hide Menu",
		function()

			Main.Visible =
				false

		end
	)

	Button(
		VisualPage,
		72,
		"🔵 Matte UI",
		function()

			Main.BackgroundTransparency =
				0.12

		end
	)

	--==========================================================
	-- AIM
	--==========================================================

	local function VisibleTarget(
		targetCharacter,
		position
	)

		local origin =
			Camera.CFrame.Position

		local direction =
			position
			-
			origin

		local params =
			RaycastParams.new()

		params.FilterType =
			Enum.RaycastFilterType.Exclude

		params.FilterDescendantsInstances =
			{
				Char()
			}

		params.IgnoreWater =
			true

		local result =
			workspace:
				Raycast(
					origin,
					direction,
					params
				)

		if not result then
			return true
		end

		return result.Instance:
			IsDescendantOf(
				targetCharacter
			)

	end

	local function FindAimTarget()

		local v =
			Camera.ViewportSize

		local center =
			Vector2.new(
				v.X/2,
				v.Y/2
			)

		local best

		local bestScreenDistance =
			S.aimFOV

		local myRoot =
			Root()

		for _,p in ipairs(
			Players:GetPlayers()
		) do

			--------------------------------------------------
			-- IMPORTANT:
			-- FRIENDS ARE IGNORED
			--------------------------------------------------

			if p ~= LP
				and
				not IsFriend(p)
				and
				p.Character
			then

				local h =
					p.Character:
						FindFirstChildOfClass(
							"Humanoid"
						)

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				local head =
					p.Character:
						FindFirstChild(
							"Head"
						)

				if h
					and
					h.Health > 0
					and
					r
					and
					head
				then

					local worldDistance =
						(
							r.Position
							-
							myRoot.Position
						).Magnitude

					if worldDistance
						<=
						S.aimMaxDistance
					then

						local screen,
						onScreen =
							Camera:
								WorldToViewportPoint(
									head.Position
								)

						if onScreen
							and
							screen.Z > 0
						then

							local distance =
								(
									Vector2.new(
										screen.X,
										screen.Y
									)
									-
									center
								).Magnitude

							if distance
								<
								bestScreenDistance

								and

								VisibleTarget(
									p.Character,
									head.Position
								)
							then

								best =
									p

								bestScreenDistance =
									distance

							end

						end

					end

				end

			end

		end

		return best

	end

	local AimButton

	AimButton =
		Button(
			VisualPage,
			nil,
			"🎯 AUTO AIM: OFF",
			function(b)

				S.aimEnabled =
					not
					S.aimEnabled

				S.aimTarget =
					nil

				S.cameraLock =
					false

				cameraTarget =
					nil

				ToggleStyle(
					b,
					S.aimEnabled,
					"🎯 AUTO AIM"
				)

			end
		)

	AimButton.LayoutOrder =
		73

	local AimPlus =
		Button(
			VisualPage,
			nil,
			"🎯 AIM AREA +50",
			function()

				S.aimFOV =
					math.min(
						1000,
						S.aimFOV + 50
					)

			end
		)

	AimPlus.LayoutOrder =
		74

	local AimMinus =
		Button(
			VisualPage,
			nil,
			"🎯 AIM AREA -50",
			function()

				S.aimFOV =
					math.max(
						50,
						S.aimFOV - 50
					)

			end
		)

	AimMinus.LayoutOrder =
		75

	local FriendInfo =
		Button(
			VisualPage,
			nil,
			"🟢 FRIENDS: 0 • СКМ ПО ИГРОКУ",
			function()
			end
		)

	FriendInfo.LayoutOrder =
		76

	local function UpdateFriendButton()

		FriendInfo.Text =
			"🟢 FRIENDS: "
			..
			FriendCount()
			..
			" • СКМ ПО ИГРОКУ"

	end

	----------------------------------------------------------
	-- AIM LOOP
	----------------------------------------------------------

	RunService:
		UnbindFromRenderStep(
			"SuperUniV11_Aim"
		)

	RunService:
		BindToRenderStep(

			"SuperUniV11_Aim",

			Enum.RenderPriority.Camera.Value
			+
			1,

			function()

				if not alive
					or
					not S.aimEnabled
					or
					not Camera
				then
					return
				end

				local chosen =
					FindAimTarget()

				S.aimTarget =
					chosen

				if not chosen
					or
					not chosen.Character
				then
					return
				end

				if IsFriend(
					chosen
				) then

					S.aimTarget =
						nil

					return

				end

				local humanoid =
					chosen.Character:
						FindFirstChildOfClass(
							"Humanoid"
						)

				local head =
					chosen.Character:
						FindFirstChild(
							"Head"
						)

				if not humanoid
					or
					humanoid.Health <= 0
					or
					not head
				then

					S.aimTarget =
						nil

					return

				end

				local cameraPosition =
					Camera.CFrame.Position

				if (
					head.Position
					-
					cameraPosition
				).Magnitude
					<
					0.01
				then
					return
				end

				--------------------------------------------------
				-- CAMERA CENTER TO TARGET
				--------------------------------------------------

				Camera.CFrame =
					CFrame.lookAt(
						cameraPosition,
						head.Position,
						Vector3.yAxis
					)

			end
		)

	--==========================================================
	-- PLAYER VISUAL HOOK
	--==========================================================

	local function HookPlayer(
		p
	)

		if p == LP then
			return
		end

		p.CharacterAdded:
		Connect(function()

			task.wait(
				0.5
			)

			if alive
				and
				(
					S.esp
					or
					S.names
					or
					S.distance
				)
			then

				RefreshVisual()

			end

		end)

	end

	for _,p in ipairs(
		Players:GetPlayers()
	) do

		HookPlayer(
			p
		)

	end

	Players.PlayerAdded:
	Connect(
			HookPlayer
		)

	----------------------------------------------------------
	-- DISTANCE UPDATE
	----------------------------------------------------------

	RunService.RenderStepped:
	Connect(function()

		if not alive
			or
			not S.distance
		then
			return
		end

		local character =
			LP.Character

		if not character then
			return
		end

		local myRoot =
			character:
				FindFirstChild(
					"HumanoidRootPart"
				)

		if not myRoot then
			return
		end

		for _,p in ipairs(
			Players:GetPlayers()
		) do

			if p ~= LP
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				local gui =
					p.Character:
						FindFirstChild(
							"SU11_DISTANCE",
							true
						)

				if r
					and
					gui
				then

					local text =
						gui:
							FindFirstChild(
								"DistanceText"
							)

					if text then

						text.Text =
							math.floor(
								(
									r.Position
									-
									myRoot.Position
								).Magnitude
							)
							..
							" studs"

						if IsFriend(
							p
						) then

							text.TextColor3 =
								FRIEND_GREEN

						end

					end

				end

			end

		end

	end)

	--==========================================================
	-- TP POINTS
	--==========================================================

	local PointsTitle =
		Instance.new(
			"TextLabel"
		)

	PointsTitle.Size =
		UDim2.new(
			1,-PX(8),
			0,PX(34)
		)

	PointsTitle.BackgroundTransparency =
		1

	PointsTitle.Text =
		"📍 Points: 0"

	PointsTitle.TextColor3 =
		isPhone
			and
			NEON_CYAN
			or
			Color3.fromRGB(
				170,210,240
			)

	PointsTitle.TextSize =
		isPC
			and 17
			or 12

	PointsTitle.Font =
		Enum.Font.GothamBold

	PointsTitle.LayoutOrder =
		0

	PointsTitle.Parent =
		TPPage

	local function UpdatePointCount()

		local count =
			0

		for _ in pairs(
			teleportPoints
		) do

			count += 1

		end

		PointsTitle.Text =
			"📍 Points: "
			..
			count

	end

	local function CreatePointRow(
		point
	)

		local holder =
			Instance.new(
				"Frame"
			)

		holder.Size =
			UDim2.new(
				1,-PX(8),
				0,PX(88)
			)

		holder.BorderSizePixel =
			0

		holder.LayoutOrder =
			1000
			+
			point.id

		holder.Parent =
			TPPage

		Round(
			holder,
			9
		)

		if isPhone then

			holder.BackgroundColor3 =
				PHONE_PANEL

			Stroke(
				holder,
				NEON_CYAN,
				1,
				0.55
			)

		else

			holder.BackgroundColor3 =
				Color3.fromRGB(
					29,64,96
				)

		end

		local go =
			Instance.new(
				"TextButton"
			)

		go.Size =
			UDim2.new(
				1,-PX(8),
				0,PX(39)
			)

		go.Position =
			UDim2.fromOffset(
				PX(4),
				PX(4)
			)

		go.BackgroundColor3 =
			isPhone
				and
				PHONE_BUTTON_ON
				or
				Color3.fromRGB(
					38,84,123
				)

		go.BorderSizePixel =
			0

		go.Text =
			"📍 "
			..
			point.name
			..
			" → TP"

		go.TextColor3 =
			Color3.new(
				1,1,1
			)

		go.TextSize =
			isPC
				and 16
				or 11

		go.Parent =
			holder

		Round(
			go,
			7
		)

		local rename =
			Instance.new(
				"TextButton"
			)

		rename.Size =
			UDim2.new(
				0.5,-PX(6),
				0,PX(35)
			)

		rename.Position =
			UDim2.fromOffset(
				PX(4),
				PX(48)
			)

		rename.BackgroundColor3 =
			isPhone
				and
				PHONE_BUTTON
				or
				Color3.fromRGB(
					45,84,118
				)

		rename.BorderSizePixel =
			0

		rename.Text =
			"✏ Rename"

		rename.TextColor3 =
			Color3.new(
				1,1,1
			)

		rename.Parent =
			holder

		Round(
			rename,
			7
		)

		local delete =
			Instance.new(
				"TextButton"
			)

		delete.Size =
			UDim2.new(
				0.5,-PX(6),
				0,PX(35)
			)

		delete.Position =
			UDim2.new(
				0.5,PX(2),
				0,PX(48)
			)

		delete.BackgroundColor3 =
			Color3.fromRGB(
				90,53,66
			)

		delete.BorderSizePixel =
			0

		delete.Text =
			"🗑 Delete"

		delete.TextColor3 =
			Color3.new(
				1,1,1
			)

		delete.Parent =
			holder

		Round(
			delete,
			7
		)

		point.gui =
			holder

		point.nameButton =
			go

		go.Activated:
		Connect(function()

			TeleportTo(
				point.cframe
				*
				CFrame.new(
					0,3,0
				)
			)

		end)

		rename.Activated:
		Connect(function()

			local newName =
				point.name
				..
				"*"

			point.name =
				newName

			go.Text =
				"📍 "
				..
				newName
				..
				" → TP"

		end)

		delete.Activated:
		Connect(function()

			teleportPoints[
				point.id
			] =
				nil

			holder:
				Destroy()

			UpdatePointCount()

		end)

	end

	--==========================================================
	-- TP 73-96
	--==========================================================

	Button(
		TPPage,
		73,
		"📍 SAVE NEW POINT",
		function()

			pointCounter +=
				1

			local point = {

				id =
					pointCounter,

				name =
					"Point "
					..
					pointCounter,

				cframe =
					Root().CFrame
			}

			teleportPoints[
				point.id
			] =
				point

			CreatePointRow(
				point
			)

			UpdatePointCount()

		end
	)

	Button(
		TPPage,
		74,
		"🗑 CLEAR ALL POINTS",
		function()

			for _,point in pairs(
				teleportPoints
			) do

				if point.gui then

					point.gui:
						Destroy()

				end

			end

			table.clear(
				teleportPoints
			)

			UpdatePointCount()

		end
	)

	Button(
		TPPage,
		75,
		"🏠 TP Spawn",
		function()

			local spawn =
				workspace:
					FindFirstChildWhichIsA(
						"SpawnLocation",
						true
					)

			if spawn then

				TeleportTo(
					spawn.CFrame
						*
						CFrame.new(
							0,4,0
						)
				)

			end

		end
	)

	Button(
		TPPage,
		76,
		"👤 TP Nearest",
		function()

			local p =
				NearestPlayer(
					false
				)

			if p
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				if r then

					TeleportTo(
						r.CFrame
							*
							CFrame.new(
								0,0,4
							)
					)

				end

			end

		end
	)

	Button(
		TPPage,
		77,
		"⬆ Above Nearest",
		function()

			local p =
				NearestPlayer(
					false
				)

			if p
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				if r then

					TeleportTo(
						r.CFrame
							*
							CFrame.new(
								0,8,0
							)
					)

				end

			end

		end
	)

	Button(
		TPPage,
		78,
		"👣 Behind Nearest",
		function()

			local p =
				NearestPlayer(
					false
				)

			if p
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				if r then

					TeleportTo(
						r.CFrame
							*
							CFrame.new(
								0,0,5
							)
					)

				end

			end

		end
	)

	Button(
		TPPage,
		79,
		"👀 Front Nearest",
		function()

			local p =
				NearestPlayer(
					false
				)

			if p
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				if r then

					TeleportTo(
						r.CFrame
							*
							CFrame.new(
								0,0,-5
							)
					)

				end

			end

		end
	)

	Button(
		TPPage,
		80,
		"👁 Spectate Nearest",
		function()

			local p =
				NearestPlayer(
					false
				)

			if p
				and
				p.Character
			then

				local h =
					p.Character:
						FindFirstChildOfClass(
							"Humanoid"
						)

				if h then

					Camera.CameraSubject =
						h

				end

			end

		end
	)

	Button(
		TPPage,
		81,
		"👤 Stop Spectate",
		function()

			Camera.CameraSubject =
				Hum()

		end
	)

	local CameraLockButton

	CameraLockButton =
		Button(
			TPPage,
			82,
			"🎯 Camera Lock",
			function(b)

				S.cameraLock =
					not
						S.cameraLock

				S.aimEnabled =
					false

				S.aimTarget =
					nil

				cameraTarget =
					S.cameraLock
						and
						NearestPlayer(
							false
						)
						or
						nil

				ToggleStyle(
					AimButton,
					false,
					"🎯 AUTO AIM"
				)

				ToggleStyle(
					b,
					S.cameraLock,
					"82. 🎯 CAMERA LOCK"
				)

			end
		)

	Button(
		TPPage,
		83,
		"❌ Stop Camera Lock",
		function()

			S.cameraLock =
				false

			cameraTarget =
				nil

			ToggleStyle(
				CameraLockButton,
				false,
				"82. 🎯 CAMERA LOCK"
			)

		end
	)

	Button(
		TPPage,
		84,
		"🏡 Save Home",
		function()

			homeCFrame =
				Root().CFrame

		end
	)

	Button(
		TPPage,
		85,
		"🏡 TP Home",
		function()

			if homeCFrame then

				TeleportTo(
					homeCFrame
						*
						CFrame.new(
							0,3,0
						)
				)

			end

		end
	)

	Button(
		TPPage,
		86,
		"🌐 TP Origin",
		function()

			TeleportTo(
				CFrame.new(
					0,10,0
				)
			)

		end
	)

	Button(
		TPPage,
		87,
		"⬆ TP +10 Y",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						0,10,0
					)
			)

		end
	)

	Button(
		TPPage,
		88,
		"⬇ TP -10 Y",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						0,-10,0
					)
			)

		end
	)

	Button(
		TPPage,
		89,
		"➡ TP +25 X",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						25,0,0
					)
			)

		end
	)

	Button(
		TPPage,
		90,
		"⬅ TP -25 X",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						-25,0,0
					)
			)

		end
	)

	Button(
		TPPage,
		91,
		"🔼 TP +25 Z",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						0,0,25
					)
			)

		end
	)

	Button(
		TPPage,
		92,
		"🔽 TP -25 Z",
		function()

			TeleportTo(
				Root().CFrame
					+
					Vector3.new(
						0,0,-25
					)
			)

		end
	)

	Button(
		TPPage,
		93,
		"🎲 TP Random Player",
		function()

			local p =
				RandomPlayer()

			if p
				and
				p.Character
			then

				local r =
					p.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

				if r then

					TeleportTo(
						r.CFrame
							*
							CFrame.new(
								0,0,4
							)
					)

				end

			end

		end
	)

	Button(
		TPPage,
		94,
		"🎲 Spectate Random",
		function()

			local p =
				RandomPlayer()

			if p
				and
				p.Character
			then

				local h =
					p.Character:
						FindFirstChildOfClass(
							"Humanoid"
						)

				if h then

					Camera.CameraSubject =
						h

				end

			end

		end
	)

	Button(
		TPPage,
		95,
		"➡ Spectate Next",
		function()

			local list =
				Players:
					GetPlayers()

			if #list <= 1 then
				return
			end

			for _ = 1,#list do

				spectateIndex +=
					1

				if spectateIndex
					>
					#list
				then

					spectateIndex =
						1

				end

				local p =
					list[
						spectateIndex
					]

				if p ~= LP
					and
						p.Character
				then

					local h =
						p.Character:
							FindFirstChildOfClass(
								"Humanoid"
							)

					if h then

						Camera.CameraSubject =
							h

						break

					end

				end

			end

		end
	)

	Button(
		TPPage,
		96,
		"↩ Previous Position",
		function()

			if previousCFrame then

				local current =
					Root().CFrame

				Root().CFrame =
					previousCFrame

				previousCFrame =
					current

			end

		end
	)

	----------------------------------------------------------
	-- CAMERA LOCK
	----------------------------------------------------------

	RunService.RenderStepped:
	Connect(function()

		if not alive
			or
			not S.cameraLock
		then
			return
		end

		if not cameraTarget
			or
			not cameraTarget.Character
		then

			cameraTarget =
				NearestPlayer(
					false
				)

		end

		if cameraTarget
			and
			cameraTarget.Character
		then

			local head =
				cameraTarget.Character:
					FindFirstChild(
						"Head"
					)

			if head then

				Camera.CFrame =
					CFrame.lookAt(
						Camera.CFrame.Position,
						head.Position
					)

			end

		end

	end)

	--==========================================================
	-- HUD
	--==========================================================

	local HUD =
		Instance.new(
			"TextLabel"
		)

	HUD.Size =
		UDim2.fromOffset(
			isPC
			and 290
			or 205,

			isPC
			and 250
			or 175
		)

	HUD.Position =
		UDim2.new(
			1,
			isPC
			and -300
			or -210,

			0,
			5
		)

	HUD.BackgroundColor3 =
		isPhone
			and
			PHONE_BG
			or
			Color3.fromRGB(
				20,45,68
			)

	HUD.BackgroundTransparency =
		0.2

	HUD.TextColor3 =
		isPhone
			and
			NEON_CYAN
			or
			Color3.new(
				1,1,1
			)

	HUD.TextSize =
		isPC
			and 15
			or 11

	HUD.Font =
		Enum.Font.Code

	HUD.TextXAlignment =
		Enum.TextXAlignment.Left

	HUD.TextYAlignment =
		Enum.TextYAlignment.Top

	HUD.Visible =
		false

	HUD.ZIndex =
		180

	HUD.Parent =
		GUI

	Round(
		HUD,
		8
	)

	local frames =
		0

	local fps =
		0

	local lastFPS =
		os.clock()

	RunService.RenderStepped:
	Connect(function()

		if not alive then
			return
		end

		frames +=
			1

		if os.clock()
			-
			lastFPS
			>=
			1
		then

			fps =
				frames

			frames =
				0

			lastFPS =
				os.clock()

		end

		local lines =
			{}

		if S.coords
			and
			LP.Character
		then

			local p =
				Root().Position

			table.insert(
				lines,

				string.format(
					"XYZ %.1f %.1f %.1f",
					p.X,
					p.Y,
					p.Z
				)
			)

		end

		if S.fps then

			table.insert(
				lines,
				"FPS: "..fps
			)

		end

		if S.ping then

			local ping =
				"?"

			pcall(function()

				ping =
					math.floor(
						LP:GetNetworkPing()
						*
						1000
					)

			end)

			table.insert(
				lines,
				"Ping: "
				..
				ping
				..
				" ms"
			)

		end

		if S.clock then

			table.insert(
				lines,
				"Time: "
				..
				os.date(
					"%H:%M:%S"
				)
			)

		end

		if S.playerCount then

			table.insert(
				lines,
				"Players: "
				..
				#Players:GetPlayers()
			)

		end

		if S.job then

			local job =
				game.JobId

			if job == "" then

				job =
					"Studio"

			end

			table.insert(
				lines,
				"Job: "
				..
				string.sub(
					job,
					1,
					18
				)
			)

		end

		if S.health then

			table.insert(
				lines,
				"HP: "
				..
				math.floor(
					Hum().Health
				)
			)

		end

		if S.speedHUD then

			table.insert(
				lines,
				"Speed: "
				..
				math.floor(
					Hum().WalkSpeed
				)
			)

		end

		if S.gravityHUD then

			table.insert(
				lines,
				"Gravity: "
				..
				math.floor(
					workspace.Gravity
				)
			)

		end

		if S.fovHUD then

			table.insert(
				lines,
				"FOV: "
				..
				math.floor(
					Camera.FieldOfView
				)
			)

		end

		if S.aimEnabled then

			table.insert(
				lines,
				"AIM: ON"
			)

		end

		if FriendCount() > 0 then

			table.insert(
				lines,
				"Friends: "
				..
				FriendCount()
			)

		end

		HUD.Text =
			table.concat(
				lines,
				"\n"
			)

		HUD.Visible =
			#lines > 0

	end)

	--==========================================================
	-- TOOLS 97-120
	--==========================================================

	Button(
		ToolsPage,
		97,
		"📍 Coordinates",
		function(b)

			S.coords =
				not S.coords

			ToggleStyle(
				b,
				S.coords,
				"97. 📍 COORDS"
			)

		end
	)

	Button(
		ToolsPage,
		98,
		"📊 FPS",
		function(b)

			S.fps =
				not S.fps

			ToggleStyle(
				b,
				S.fps,
				"98. 📊 FPS"
			)

		end
	)

	Button(
		ToolsPage,
		99,
		"📶 Ping",
		function(b)

			S.ping =
				not S.ping

			ToggleStyle(
				b,
				S.ping,
				"99. 📶 PING"
			)

		end
	)

	Button(
		ToolsPage,
		100,
		"🕐 Clock",
		function(b)

			S.clock =
				not S.clock

			ToggleStyle(
				b,
				S.clock,
				"100. 🕐 CLOCK"
			)

		end
	)

	Button(
		ToolsPage,
		101,
		"👥 Player Count",
		function(b)

			S.playerCount =
				not
					S.playerCount

			ToggleStyle(
				b,
				S.playerCount,
				"101. 👥 PLAYERS"
			)

		end
	)

	Button(
		ToolsPage,
		102,
		"🌐 Job ID",
		function(b)

			S.job =
				not S.job

			ToggleStyle(
				b,
				S.job,
				"102. 🌐 JOB"
			)

		end
	)

	Button(
		ToolsPage,
		103,
		"❤️ Health HUD",
		function(b)

			S.health =
				not S.health

			ToggleStyle(
				b,
				S.health,
				"103. ❤️ HEALTH"
			)

		end
	)

	Button(
		ToolsPage,
		104,
		"⚡ Speed HUD",
		function(b)

			S.speedHUD =
				not S.speedHUD

			ToggleStyle(
				b,
				S.speedHUD,
				"104. ⚡ SPEED HUD"
			)

		end
	)

	Button(
		ToolsPage,
		105,
		"🌍 Gravity HUD",
		function(b)

			S.gravityHUD =
				not S.gravityHUD

			ToggleStyle(
				b,
				S.gravityHUD,
				"105. 🌍 GRAVITY"
			)

		end
	)

	Button(
		ToolsPage,
		106,
		"🎥 FOV HUD",
		function(b)

			S.fovHUD =
				not S.fovHUD

			ToggleStyle(
				b,
				S.fovHUD,
				"106. 🎥 FOV HUD"
			)

		end
	)

	Button(
		ToolsPage,
		107,
		"🎥 Reset Camera",
		function()

			S.cameraLock =
				false

			S.aimEnabled =
				false

			S.aimTarget =
				nil

			cameraTarget =
				nil

			Camera.CameraSubject =
				Hum()

			Camera.CameraType =
				Enum.CameraType.Custom

			Camera.FieldOfView =
				70

			ToggleStyle(
				AimButton,
				false,
				"🎯 AUTO AIM"
			)

			ToggleStyle(
				CameraLockButton,
				false,
				"82. 🎯 CAMERA LOCK"
			)

		end
	)

	Button(
		ToolsPage,
		108,
		"👤 Camera Self",
		function()

			Camera.CameraSubject =
				Hum()

		end
	)

	local function ResizeMain(
		w,
		h
	)

		local v =
			Camera.ViewportSize

		Main.Size =
			UDim2.fromOffset(

				math.min(
					w,
					v.X-10
				),

				math.min(
					h,
					v.Y-10
				)

			)

		ClampObject(
			Main,
			Main.AbsolutePosition.X,
			Main.AbsolutePosition.Y
		)

	end

	Button(
		ToolsPage,
		109,
		"🔹 Small Menu",
		function()

			if isPC then

				ResizeMain(
					650,
					560
				)

			else

				ResizeMain(
					270,
					340
				)

			end

		end
	)

	Button(
		ToolsPage,
		110,
		"🔷 Normal Menu",
		function()

			if isPC then

				ResizeMain(
					800,
					720
				)

			else

				ResizeMain(
					320,
					430
				)

			end

		end
	)

	Button(
		ToolsPage,
		111,
		"🔵 MAX Menu",
		function()

			local v =
				Camera.ViewportSize

			ResizeMain(
				v.X-20,
				v.Y-20
			)

		end
	)

	Button(
		ToolsPage,
		112,
		"🎯 Center Menu",
		function()

			local v =
				Camera.ViewportSize

			ClampObject(
				Main,

				(
					v.X
					-
					Main.AbsoluteSize.X
				)/2,

				(
					v.Y
					-
					Main.AbsoluteSize.Y
				)/2
			)

		end
	)

	Button(
		ToolsPage,
		113,
		"⬅ Icon Left",
		function()

			ClampObject(
				Icon,
				5,
				Icon.AbsolutePosition.Y
			)

		end
	)

	Button(
		ToolsPage,
		114,
		"➡ Icon Right",
		function()

			ClampObject(
				Icon,

				Camera.ViewportSize.X
				-
				Icon.AbsoluteSize.X
				-
				5,

				Icon.AbsolutePosition.Y
			)

		end
	)

	Button(
		ToolsPage,
		115,
		"↖ Icon Top Left",
		function()

			ClampObject(
				Icon,
				5,
				5
			)

		end
	)

	Button(
		ToolsPage,
		116,
		"↙ Icon Bottom Left",
		function()

			ClampObject(
				Icon,
				5,

				Camera.ViewportSize.Y
				-
				Icon.AbsoluteSize.Y
				-
				5
			)

		end
	)

	Button(
		ToolsPage,
		117,
		"🔵 UI Opacity 10%",
		function()

			Main.BackgroundTransparency =
				0.10

		end
	)

	Button(
		ToolsPage,
		118,
		"🔷 UI Opacity 30%",
		function()

			Main.BackgroundTransparency =
				0.30

		end
	)

	Button(
		ToolsPage,
		119,
		"🚨 PANIC RESET",
		function()

			S.infiniteJump =
				false

			S.fly =
				false

			S.flyUp =
				false

			S.flyDown =
				false

			S.noclip =
				false

			S.hover =
				false

			S.spin =
				false

			S.god =
				false

			S.platformStand =
				false

			S.cameraLock =
				false

			S.aimEnabled =
				false

			S.aimTarget =
				nil

			cameraTarget =
				nil

			workspace.Gravity =
				originalGravity

			local h =
				Hum()

			h.WalkSpeed =
				16

			h.UseJumpPower =
				true

			h.JumpPower =
				50

			h.AutoRotate =
				true

			h.PlatformStand =
				false

			h.MaxHealth =
				normalMaxHealth

			h.Health =
				math.min(
					h.Health,
					normalMaxHealth
				)

			Root().Anchored =
				false

			Root().AssemblyAngularVelocity =
				Vector3.zero

			for part,value in pairs(
				collisionCache
			) do

				if part
					and
					part.Parent
				then

					part.CanCollide =
						value

				end

			end

			table.clear(
				collisionCache
			)

			Lighting.Brightness =
				originalBrightness

			Lighting.ClockTime =
				originalClock

			Lighting.FogStart =
				originalFogStart

			Lighting.FogEnd =
				originalFogEnd

			Lighting.Ambient =
				originalAmbient

			Lighting.OutdoorAmbient =
				originalOutdoor

			Camera.CameraSubject =
				h

			Camera.CameraType =
				Enum.CameraType.Custom

			Camera.FieldOfView =
				70

			RemoveDecoy()

			if forceField then

				forceField:
					Destroy()

				forceField =
					nil

			end

			ToggleStyle(
				FlyButton,
				false,
				"25. ✈ FLY"
			)

			ToggleStyle(
				NoclipButton,
				false,
				"28. 👻 NOCLIP"
			)

			ToggleStyle(
				AimButton,
				false,
				"🎯 AUTO AIM"
			)

			ToggleStyle(
				CameraLockButton,
				false,
				"82. 🎯 CAMERA LOCK"
			)

		end
	)

	Button(
		ToolsPage,
		120,
		"❌ Close SuperUni",
		function()

			alive =
				false

			S.aimEnabled =
				false

			RunService:
				UnbindFromRenderStep(
					"SuperUniV11_Aim"
				)

			if isPC then

				pcall(function()

					StarterGui:
						SetCoreGuiEnabled(
							Enum.CoreGuiType.PlayerList,
							true
						)

				end)

			end

			GUI:
				Destroy()

		end
	)

	--==========================================================
	-- HOTKEY EDITOR
	--==========================================================

	local waitingForHotkey

	local hotkeyButtons =
		{}

	local hotkeyLabels = {

		Menu =
			"📋 MENU",

		Fly =
			"✈ FLY",

		FlyUp =
			"⬆ FLY UP",

		FlyDown =
			"⬇ FLY DOWN",

		Noclip =
			"👻 NOCLIP",

		Aim =
			"🎯 AUTO AIM",

		ESP =
			"👁 ESP",

		Speed =
			"⚡ SPEED",

		Heal =
			"❤️ HEAL"
	}

	local function HotkeyButton(
		action
	)

		local b

		b =
			Button(
				HotkeyPage,
				nil,

				hotkeyLabels[action]
				..
				": "
				..
				Hotkeys[action].Name,

				function()

					waitingForHotkey =
						action

					b.Text =
						"⌨ Нажми новую клавишу..."

				end
			)

		hotkeyButtons[action] =
			b

	end

	HotkeyButton(
		"Menu"
	)

	HotkeyButton(
		"Fly"
	)

	HotkeyButton(
		"FlyUp"
	)

	HotkeyButton(
		"FlyDown"
	)

	HotkeyButton(
		"Noclip"
	)

	HotkeyButton(
		"Aim"
	)

	HotkeyButton(
		"ESP"
	)

	HotkeyButton(
		"Speed"
	)

	HotkeyButton(
		"Heal"
	)

	Button(
		HotkeyPage,
		nil,
		"♻ RESET DEFAULT KEYS",
		function()

			Hotkeys.Menu =
				Enum.KeyCode.Tab

			Hotkeys.Fly =
				Enum.KeyCode.F

			Hotkeys.FlyUp =
				Enum.KeyCode.Eight

			Hotkeys.FlyDown =
				Enum.KeyCode.Two

			Hotkeys.Noclip =
				Enum.KeyCode.N

			Hotkeys.Aim =
				Enum.KeyCode.Q

			Hotkeys.ESP =
				Enum.KeyCode.E

			Hotkeys.Speed =
				Enum.KeyCode.V

			Hotkeys.Heal =
				Enum.KeyCode.H

			for action,b in pairs(
				hotkeyButtons
			) do

				b.Text =
					hotkeyLabels[action]
					..
					": "
					..
					Hotkeys[action].Name

			end

		end
	)

	----------------------------------------------------------
	-- KEY MATCH
	----------------------------------------------------------

	local function KeyMatches(
		action,
		key
	)

		if key
			==
			Hotkeys[action]
		then
			return true
		end

		if action
			==
			"FlyUp"

			and

			Hotkeys.FlyUp
			==
			Enum.KeyCode.Eight

			and

			key
			==
			Enum.KeyCode.KeypadEight
		then

			return true

		end

		if action
			==
			"FlyDown"

			and

			Hotkeys.FlyDown
			==
			Enum.KeyCode.Two

			and

			key
			==
			Enum.KeyCode.KeypadTwo
		then

			return true

		end

		return false

	end

	--==========================================================
	-- INPUT
	--==========================================================

	UIS.InputBegan:
	Connect(function(
		input,
		processed
	)

		if not alive then
			return
		end

		--------------------------------------------------
		-- MIDDLE MOUSE = FRIEND
		--------------------------------------------------

		if input.UserInputType
			==
			Enum.UserInputType.MouseButton3
		then

			local targetPart =
				Mouse.Target

			local player =
				PlayerFromPart(
					targetPart
				)

			if player
				and
				player ~= LP
			then

				ToggleFriend(
					player
				)

				UpdateFriendButton()

			end

			return

		end

		if UIS:GetFocusedTextBox() then
			return
		end

		--------------------------------------------------
		-- REBIND
		--------------------------------------------------

		if waitingForHotkey then

			if input.UserInputType
				==
				Enum.UserInputType.Keyboard

				and

				input.KeyCode
				~=
				Enum.KeyCode.Unknown
			then

				local action =
					waitingForHotkey

				if input.KeyCode
					==
					Enum.KeyCode.Escape
				then

					waitingForHotkey =
						nil

					hotkeyButtons[action].Text =
						hotkeyLabels[action]
						..
						": "
						..
						Hotkeys[action].Name

					return

				end

				Hotkeys[action] =
					input.KeyCode

				waitingForHotkey =
					nil

				hotkeyButtons[action].Text =
					hotkeyLabels[action]
					..
					": "
					..
					Hotkeys[action].Name

			end

			return

		end

		--------------------------------------------------
		-- MENU
		--------------------------------------------------

		if KeyMatches(
			"Menu",
			input.KeyCode
		) then

			Main.Visible =
				not
					Main.Visible

			return

		end

		if processed then
			return
		end

		--------------------------------------------------
		-- FLY
		--------------------------------------------------

		if KeyMatches(
			"Fly",
			input.KeyCode
		) then

			S.fly =
				not
					S.fly

			if not S.fly then

				S.flyUp =
					false

				S.flyDown =
					false

			end

			ToggleStyle(
				FlyButton,
				S.fly,
				"25. ✈ FLY"
			)

			return

		end

		if KeyMatches(
			"FlyUp",
			input.KeyCode
		) then

			if S.fly then

				S.flyUp =
					true

				S.flyDown =
					false

			end

			return

		end

		if KeyMatches(
			"FlyDown",
			input.KeyCode
		) then

			if S.fly then

				S.flyDown =
					true

				S.flyUp =
					false

			end

			return

		end

		--------------------------------------------------
		-- NOCLIP
		--------------------------------------------------

		if KeyMatches(
			"Noclip",
			input.KeyCode
		) then

			S.noclip =
				not
					S.noclip

			ToggleStyle(
				NoclipButton,
				S.noclip,
				"28. 👻 NOCLIP"
			)

			return

		end

		--------------------------------------------------
		-- AIM
		--------------------------------------------------

		if KeyMatches(
			"Aim",
			input.KeyCode
		) then

			S.aimEnabled =
				not
					S.aimEnabled

			S.aimTarget =
				nil

			S.cameraLock =
				false

			cameraTarget =
				nil

			ToggleStyle(
				AimButton,
				S.aimEnabled,
				"🎯 AUTO AIM"
			)

			ToggleStyle(
				CameraLockButton,
				false,
				"82. 🎯 CAMERA LOCK"
			)

			return

		end

		--------------------------------------------------
		-- ESP
		--------------------------------------------------

		if KeyMatches(
			"ESP",
			input.KeyCode
		) then

			S.esp =
				not S.esp

			RefreshVisual()

			ToggleStyle(
				ESPButton,
				S.esp,
				"49. 👁 ESP"
			)

			return

		end

		--------------------------------------------------
		-- SPEED
		--------------------------------------------------

		if KeyMatches(
			"Speed",
			input.KeyCode
		) then

			if Hum().WalkSpeed
				==
				60
			then

				Hum().WalkSpeed =
					16

			else

				Hum().WalkSpeed =
					60

			end

			return

		end

		--------------------------------------------------
		-- HEAL
		--------------------------------------------------

		if KeyMatches(
			"Heal",
			input.KeyCode
		) then

			Hum().Health =
				Hum().MaxHealth

			return

		end

	end)

	UIS.InputEnded:
	Connect(function(input)

		if not alive then
			return
		end

		if KeyMatches(
			"FlyUp",
			input.KeyCode
		) then

			S.flyUp =
				false

		end

		if KeyMatches(
			"FlyDown",
			input.KeyCode
		) then

			S.flyDown =
				false

		end

	end)

	--==========================================================
	-- NOCLIP
	--==========================================================

	RunService.Stepped:
	Connect(function()

		if not alive then
			return
		end

		if S.noclip then

			for _,part in ipairs(
				Char():
					GetDescendants()
			) do

				if part:IsA(
					"BasePart"
				) then

					if collisionCache[
						part
					]
						==
						nil
					then

						collisionCache[
							part
						] =
							part.CanCollide

					end

					part.CanCollide =
						false

				end

			end

		else

			for part,value in pairs(
				collisionCache
			) do

				if part
					and
					part.Parent
				then

					part.CanCollide =
						value

				end

			end

			table.clear(
				collisionCache
			)

		end

	end)

	--==========================================================
	-- FLY / GOD / SPIN
	--==========================================================

	RunService.RenderStepped:
	Connect(function()

		if not alive then
			return
		end

		local character =
			LP.Character

		if not character then
			return
		end

		local r =
			character:
				FindFirstChild(
					"HumanoidRootPart"
				)

		local h =
			character:
				FindFirstChildOfClass(
					"Humanoid"
				)

		if not r
			or
			not h
		then
			return
		end

		if S.fly then

			local move =
				h.MoveDirection

			local y =
				0

			if S.flyUp then
				y += 45
			end

			if S.flyDown then
				y -= 45
			end

			r.AssemblyLinearVelocity =
				Vector3.new(
					move.X * 58,
					y,
					move.Z * 58
				)

		elseif S.hover then

			r.AssemblyLinearVelocity =
				Vector3.new(

					r.AssemblyLinearVelocity.X,

					0,

					r.AssemblyLinearVelocity.Z

				)

		end

		if S.spin then

			r.AssemblyAngularVelocity =
				Vector3.new(
					0,8,0
				)

		end

		if S.god then

			h.MaxHealth =
				1000000000

			h.Health =
				h.MaxHealth

		end

	end)

	--==========================================================
	-- RESPAWN
	--==========================================================

	LP.CharacterAdded:
	Connect(function(character)

		task.wait(
			0.7
		)

		if not alive then
			return
		end

		table.clear(
			collisionCache
		)

		local h =
			character:
				FindFirstChildOfClass(
					"Humanoid"
				)

		if h then

			normalMaxHealth =
				h.MaxHealth

		end

		if S.god then

			CreateDecoy()

			Hum().MaxHealth =
				1000000000

			Hum().Health =
				Hum().MaxHealth

		end

		if S.esp
			or
			S.names
			or
			S.distance
		then

			task.wait(
				0.4
			)

			RefreshVisual()

		end

	end)

	print(
		"⚡ SuperUni V11 loaded"
	)

end

--==============================================================
-- ACCESS GUI
--==============================================================

local AccessGUI =
	Instance.new(
		"ScreenGui"
	)

AccessGUI.Name =
	"SuperUniAccessV11"

AccessGUI.ResetOnSpawn =
	false

AccessGUI.IgnoreGuiInset =
	true

AccessGUI.DisplayOrder =
	999999

AccessGUI.Parent =
	PG

local AccessFrame =
	Instance.new(
		"Frame"
	)

AccessFrame.Size =
	UDim2.fromOffset(
		300,
		320
	)

AccessFrame.Position =
	UDim2.new(
		0.5,-150,
		0.5,-160
	)

AccessFrame.BackgroundColor3 =
	Color3.fromRGB(
		22,49,75
	)

AccessFrame.BorderSizePixel =
	0

AccessFrame.Parent =
	AccessGUI

local AccessCorner =
	Instance.new(
		"UICorner"
	)

AccessCorner.CornerRadius =
	UDim.new(
		0,
		15
	)

AccessCorner.Parent =
	AccessFrame

local AccessStroke =
	Instance.new(
		"UIStroke"
	)

AccessStroke.Color =
	Color3.fromRGB(
		90,155,210
	)

AccessStroke.Thickness =
	2

AccessStroke.Transparency =
	0.3

AccessStroke.Parent =
	AccessFrame

--------------------------------------------------------------
-- TITLE
--------------------------------------------------------------

local AccessTitle =
	Instance.new(
		"TextLabel"
	)

AccessTitle.Size =
	UDim2.new(
		1,0,
		0,50
	)

AccessTitle.BackgroundTransparency =
	1

AccessTitle.Text =
	"⚡ SuperUni V11"

AccessTitle.TextColor3 =
	Color3.new(
		1,1,1
	)

AccessTitle.TextSize =
	22

AccessTitle.Font =
	Enum.Font.GothamBold

AccessTitle.Parent =
	AccessFrame

--------------------------------------------------------------
-- DEVICE TEXT
--------------------------------------------------------------

local DeviceText =
	Instance.new(
		"TextLabel"
	)

DeviceText.Size =
	UDim2.new(
		1,-20,
		0,22
	)

DeviceText.Position =
	UDim2.fromOffset(
		10,48
	)

DeviceText.BackgroundTransparency =
	1

DeviceText.Text =
	"Выберите устройство"

DeviceText.TextColor3 =
	Color3.fromRGB(
		175,205,230
	)

DeviceText.TextSize =
	12

DeviceText.Parent =
	AccessFrame

--------------------------------------------------------------
-- PHONE
--------------------------------------------------------------

local Phone =
	Instance.new(
		"TextButton"
	)

Phone.Size =
	UDim2.new(
		0.5,-15,
		0,40
	)

Phone.Position =
	UDim2.fromOffset(
		10,75
	)

Phone.BackgroundColor3 =
	DEVICE_MODE == "PHONE"
		and
		Color3.fromRGB(
			42,105,155
		)
		or
		Color3.fromRGB(
			31,67,99
		)

Phone.BorderSizePixel =
	0

Phone.Text =
	"📱 ТЕЛЕФОН"

Phone.TextColor3 =
	Color3.new(
		1,1,1
	)

Phone.TextSize =
	12

Phone.Font =
	Enum.Font.GothamBold

Phone.Parent =
	AccessFrame

local PhoneCorner =
	Instance.new(
		"UICorner"
	)

PhoneCorner.CornerRadius =
	UDim.new(
		0,8
	)

PhoneCorner.Parent =
	Phone

--------------------------------------------------------------
-- PC
--------------------------------------------------------------

local PC =
	Instance.new(
		"TextButton"
	)

PC.Size =
	UDim2.new(
		0.5,-15,
		0,40
	)

PC.Position =
	UDim2.new(
		0.5,5,
		0,75
	)

PC.BackgroundColor3 =
	DEVICE_MODE == "PC"
		and
		Color3.fromRGB(
			42,105,155
		)
		or
		Color3.fromRGB(
			31,67,99
		)

PC.BorderSizePixel =
	0

PC.Text =
	"💻 КОМПЬЮТЕР"

PC.TextColor3 =
	Color3.new(
		1,1,1
	)

PC.TextSize =
	12

PC.Font =
	Enum.Font.GothamBold

PC.Parent =
	AccessFrame

local PCCorner =
	Instance.new(
		"UICorner"
	)

PCCorner.CornerRadius =
	UDim.new(
		0,8
	)

PCCorner.Parent =
	PC

Phone.Activated:
Connect(function()

	DEVICE_MODE =
		"PHONE"

	Phone.BackgroundColor3 =
		Color3.fromRGB(
			42,105,155
		)

	PC.BackgroundColor3 =
		Color3.fromRGB(
			31,67,99
		)

end)

PC.Activated:
Connect(function()

	DEVICE_MODE =
		"PC"

	PC.BackgroundColor3 =
		Color3.fromRGB(
			42,105,155
		)

	Phone.BackgroundColor3 =
		Color3.fromRGB(
			31,67,99
		)

end)

--------------------------------------------------------------
-- PASSWORD BOX
--------------------------------------------------------------

local Box =
	Instance.new(
		"TextBox"
	)

Box.Size =
	UDim2.new(
		1,-20,
		0,42
	)

Box.Position =
	UDim2.fromOffset(
		10,125
	)

Box.BackgroundColor3 =
	Color3.fromRGB(
		31,66,98
	)

Box.BorderSizePixel =
	0

Box.Text =
	""

Box.PlaceholderText =
	"Premium пароль..."

Box.TextColor3 =
	Color3.new(
		1,1,1
	)

Box.PlaceholderColor3 =
	Color3.fromRGB(
		155,190,220
	)

Box.TextSize =
	14

Box.ClearTextOnFocus =
	false

Box.Parent =
	AccessFrame

--------------------------------------------------------------
-- PREMIUM
--------------------------------------------------------------

local Premium =
	Instance.new(
		"TextButton"
	)

Premium.Size =
	UDim2.new(
		1,-20,
		0,42
	)

Premium.Position =
	UDim2.fromOffset(
		10,175
	)

Premium.BackgroundColor3 =
	Color3.fromRGB(
		42,98,145
	)

Premium.BorderSizePixel =
	0

Premium.Text =
	"🔑 PREMIUM ВХОД"

Premium.TextColor3 =
	Color3.new(
		1,1,1
	)

Premium.TextSize =
	13

Premium.Font =
	Enum.Font.GothamBold

Premium.Parent =
	AccessFrame

--------------------------------------------------------------
-- FREE
--------------------------------------------------------------

local Free =
	Instance.new(
		"TextButton"
	)

Free.Size =
	UDim2.new(
		1,-20,
		0,42
	)

Free.Position =
	UDim2.fromOffset(
		10,225
	)

Free.BackgroundColor3 =
	Color3.fromRGB(
		37,120,125
	)

Free.BorderSizePixel =
	0

Free.Text =
	"🎁 FREE ACCESS — 24H"

Free.TextColor3 =
	Color3.new(
		1,1,1
	)

Free.TextSize =
	13

Free.Font =
	Enum.Font.GothamBold

Free.Parent =
	AccessFrame

--------------------------------------------------------------
-- STATUS
--------------------------------------------------------------

local Status =
	Instance.new(
		"TextLabel"
	)

Status.Size =
	UDim2.new(
		1,-20,
		0,30
	)

Status.Position =
	UDim2.fromOffset(
		10,275
	)

Status.BackgroundTransparency =
	1

Status.Text =
	""

Status.TextColor3 =
	Color3.fromRGB(
		180,210,235
	)

Status.TextSize =
	11

Status.Parent =
	AccessFrame

--------------------------------------------------------------
-- OPEN
--------------------------------------------------------------

local opened =
	false

local function Open()

	if opened then
		return
	end

	opened =
		true

	AccessGUI:
		Destroy()

	StartSuperUni(
		DEVICE_MODE
	)

end

--------------------------------------------------------------
-- PREMIUM LOGIN
--------------------------------------------------------------

Premium.Activated:
Connect(function()

	if Box.Text
		==
		PASSWORD
	then

		Premium.Text =
			"✅ PREMIUM"

		task.wait(
			0.15
		)

		Open()

	else

		Box.Text =
			""

		Box.PlaceholderText =
			"❌ Неверный пароль"

	end

end)

--------------------------------------------------------------
-- FREE LOGIN
--------------------------------------------------------------

Free.Activated:
Connect(function()

	if FreeValid() then

		Open()

		return

	end

	if LP:GetAttribute(
		"SuperUniFreeUsed"
	) then

		Free.Text =
			"❌ FREE УЖЕ ИСПОЛЬЗОВАН"

		return

	end

	if ActivateFree() then

		Free.Text =
			"✅ FREE 24H"

		task.wait(
			0.15
		)

		Open()

	end

end)

--------------------------------------------------------------
-- ENTER
--------------------------------------------------------------

Box.FocusLost:
Connect(function(enter)

	if enter
		and
		Box.Text == PASSWORD
	then

		Open()

	end

end)

--------------------------------------------------------------
-- FREE TIMER
--------------------------------------------------------------

task.spawn(function()

	while AccessGUI.Parent do

		if FreeValid() then

			local expires =
				LP:GetAttribute(
					"SuperUniFreeExpires"
				)

			local left =
				math.max(
					0,
					expires
					-
					os.time()
				)

			local h =
				math.floor(
					left/3600
				)

			local m =
				math.floor(
					(left%3600)/60
				)

			local s =
				left%60

			Status.Text =
				string.format(
					"FREE: %02d:%02d:%02d",
					h,m,s
				)

		elseif LP:GetAttribute(
			"SuperUniFreeUsed"
		) then

			Status.Text =
				"FREE уже использован"

		else

			Status.Text =
				"FREE доступен 1 раз"

		end

		task.wait(
			1
		)

	end

end)

print(
	"⚡ SuperUni V11 Access loaded"
)
