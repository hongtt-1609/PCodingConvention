# DotNet Security

Phần này bao gồm một tập hợp các quy tắc về bảo mật trong DotNet

## 1. Mã hóa
- Sử dụng API bảo vệ dữ liệu của Windows (DPAPI) để lưu trữ cục bộ dữ liệu nhạy cảm.
- Mã hóa password bằng một trong các thuật toán băm như: SHA512, Rfc2898DeriveBytes, PBKDF2.
- Sử dụng Nuget để cập nhật các package. Không nên sử dụng từ file dll.

## 2. Truy cập dữ liệu
- Không sử dụng câu sql với tham số được nối chuỗi.

## 3. Quy ước bảo mật web
#### Quy tắc chung
- Luôn sử dụng HTTPS
- Triển khai customErrors.
- Xóa server version trong response header(sử dụng Url Rewrite, hoặc config trong web.config)
- Không vô hiệu hóa validateRequest trong web.config.
- Xác thực URI bằng Uri.IsWellFormedUriString.
- Sử dụng account kết nối database có quyền truy cập là tối thiểu.
- Ghi log đầy đủ

#### Đảm bảo cookie được gửi qua httpOnly
```csharp
	CookieHttpOnly = true,
```

#### Đảm bảo cookie được gửi qua HTTPS ở môi trường product
```csharp
	<httpCookies requireSSL="true" xdt:Transform="SetAttributes(requireSSL)"/>
	<authentication>
		<forms requireSSL="true" xdt:Transform="SetAttributes(requireSSL)"/>
	</authentication>
```

#### Bảo vệ phương thức login chống lại các cuộc tấn công Brute Force
```csharp
	[HttpPost]
	[AllowAnonymous]
	[ValidateAntiForgeryToken]
	[AllowXRequestsEveryXSecondsAttribute(Name = "LogOn",
	Message = "You have performed this action more than {x} times in the last {n} seconds.",
	Requests = 3, Seconds = 60)]
	public async Task<ActionResult> LogOn(LogOnViewModel model, string returnUrl)
```

#### Đảm bảo trình gỡ lỗi và theo dõi (debug and trace) được tắt trong môi trường product
```csharp
	<compilation xdt:Transform="RemoveAttributes(debug)" />
	<trace enabled="false" xdt:Transform="Replace"/>
```

#### Sử dụng validating Anti-Forgery-Tokens đối với các form dạng POST
```csharp
	//View
	using (Html.BeginForm("LogOff", "Account", FormMethod.Post, new { id = "logoutForm"}))
	{
		@Html.AntiForgeryToken()
		//...
	}
	
	//Controller
	[HttpPost]
	[ValidateAntiForgeryToken]
	public ActionResult LogOff()
```

#### Sử dụng AntiXssEncoder để ngăn chặn tấn công XSS.
```csharp
	<system.web>
	<httpRuntime targetFramework="4.5" enableVersionHeader="false"
		encoderType="Microsoft.Security.Application.AntiXssEncoder, AntiXssLibrary" maxRequestLength="4096" />
```

#### Nên cho phép chính sách bảo mật nội dung(Content Security Policy)
```csharp
	<system.webServer>
	<httpProtocol>
        <customHeaders>
            <add name="Content-Security-Policy"
                value="default-src 'none'; style-src 'self'; img-src 'self'; font-src 'self'; script-src 'self'"/>
```

#### Chuyển hướng và chuyển tiếp phải được xác thực
```csharp
	private ActionResult RedirectToLocal(string returnUrl)
	{
		if (Url.IsLocalUrl(returnUrl))
		{
			return Redirect(returnUrl);
		}
		else
		{
			return RedirectToAction("Landing", "Account");
		}
	}	
```


