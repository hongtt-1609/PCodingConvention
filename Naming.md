DotNet Naming Convertion
=====================

Phần này bao gồm một tập hợp các quy tắc đặt tên trong dự án DotNet

## Khái quát chung
| Đối tượng                 | Ký hiệu    | Độ dài | Số nhiều | Tiền tố | Hậu tố | Viết tắt	| Các ký tự cho phép | Dấu gạch dưới|
|:--------------------------|:-----------|-------:|:---------|:--------|:-------|:----------|:-------------------|:-------------|
| Class name                | PascalCase |    128 | No       | No      | Yes    | No        | [A-z][0-9]         | No           |
| Constructor name          | PascalCase |    128 | No       | No      | Yes    | No        | [A-z][0-9]         | No           |
| Method name               | PascalCase |    128 | Yes      | No      | No     | No        | [A-z][0-9]         | No           |
| Method arguments          | camelCase  |    128 | Yes      | No      | No     | Yes       | [A-z][0-9]         | No           |
| Local variables           | camelCase  |     50 | Yes      | No      | No     | Yes       | [A-z][0-9]         | No           |
| Constants name            | PascalCase |     50 | No       | No      | No     | No        | [A-z][0-9]         | No           |
| Field name                | camelCase  |     50 | Yes      | No      | No     | Yes       | [A-z][0-9]         | Yes          |
| Properties name           | PascalCase |     50 | Yes      | No      | No     | Yes       | [A-z][0-9]         | No           |
| Delegate name             | PascalCase |    128 | No       | No      | Yes    | Yes       | [A-z]              | No           |
| Enum type name            | PascalCase |    128 | Yes      | No      | No     | No        | [A-z]              | No           |

## 1. Ký Hiệu (Notation)
- "P" = PascalCase là viết hoa tất cả các chữ cái đầu.
- "c" = camelCase từ đầu tiên bắt đầu bằng chữ thường, các từ tiếp theo viết hoa chữ cái đầu.
- "U" = Uppercase tất cả các ký tự trong từ định danh phải được viết hoa và phân cách nhau bởi dấu gạch dưới
- "_" = Tiền tố là dấu gạch dưới
- "x" = Không áp dụng
- underscore sử dụng dấu gạch chân giữa các từ, tất cả các từ đều viết thường.
- Hungarian bắt đầu tên biến thì viết chữ thường và các chữ đầu thể hiện kiểu dữ liệu của biến, và được gọi là các tiền tố

|Identifier       |Public |Protected |Internal |Private |Notes                                        |
|:----------------|:-----:|:--------:|:-------:|:------:|:--------------------------------------------|
|Project File     |P      |x         |x        |x       |Match Assembly & Namespace.                  |
|Source File      |P      |x         |x        |x       |Match contained class.                       |
|Other Files      |P      |x         |x        |x       |Apply where possible.                        |
|Namespace        |P      |x         |x        |x       |Partial Project/Assembly match.              |
|Class or Struct  |P      |P         |P        |P       |Add suffix of	subclass.                     |
|Interface        |P      |P         |P        |P       |Prefix with a capital I.                     |
|Generic Class    |P      |P         |P        |P       |Use T or K as Type identifier.               |
|Method           |P      |P         |P        |P       |Use a Verb or Verb-Object pair.              |
|Property         |P      |P         |P        |P       |Do not prefix with Get or Set.               |
|Enum             |P      |P         |P        |P       |Options are also PascalCase.                 |
|Event            |P      |P         |P        |P       |                                             |
|Field            |P      |P         |P        |_c      |Only use Private fields. No Hungarian.       |
|Static Field     |P      |P         |P        |_c      |Only use Private fields.                     |
|Constant         |U      |U         |U        |U       |                                             |
|Resource         |U      |U         |U        |U       |                                             |
|Inline Variable  |x      |x         |x        |c       |Avoid single-character and enumerated name.  |
|Parameter        |x      |x         |x        |c       |                                             |

#### Sử dụng ký hiệu kiểu PascalCase cho tên method, class, struct, enum
```csharp
public class SportProduct
{
  public void GetSportProductById()
  {
    //...
  }
  public void UpdateSportProduct()
  {
    //...
  }
}
```

#### Sử dụng ký hiệu kiểu camelCasing cho tên tham số, tên thuộc tính và biến local
```csharp
public class SportProduct
{
  public void GetSportProductById(int productId)
  {
	int productCategory = 10;
    //...
  }
}
```

#### Không sử dụng ký hiệu Hungarian hoặc bất kì loại nhận dạng đối với các định danh(identifiers)
```csharp
// Correct
int counter;
string studentName;    
// Avoid
int iCounter;
string strStudentName;
```

#### Sử dụng kí hiệu PascalCase cho các biến readonly
```csharp
public const string ShippingType = "DropShip";
```

#### Sử dụng kí hiệu Uppercase cho các biến hằng số, biến ở trong resource file
```csharp
public const int NUMBER_OF_STUDENT = 10;
```

### Ngoại lệ đối với một số từ ghép dạng closed-form như:
BitFlag, Callback, Canceled, DoNot, Email, Endpoint, FileName, Gridline, Hashtable, Id, Indexes, LogOff, LogOn, Metadata, Multipanel, Multiview, Namespace, Ok, Pi, Placeholder, SignIn, SignOut, UserName, WhiteSpace, Writable.

## 2. Độ dài (Length)
- Không nên đặt quá dài và tên phải mang ý nghĩa
- Có thể xem thêm ở phần [Khái quát chung](Naming.md#khái-quát-chung)

## 3. Số nhiều(Plural - thêm 's' ở cuối)
- Không sử dụng cho  cho tên class, struct, enum, const
- Có thể sử dụng cho tên biến, method
```csharp
  public int Sum(params int[] args)
  {
	return args.Sum();
  }
```

## 4. Tiền tố
#### Bắt buộc đặt tiền tế cho các biến là control
| Control                       | Tiền tố    | Ví dụ  			| Control                       | Tiền tố    | Ví dụ  			|
|:------------------------------|:-----------|:-----------------|:------------------------------|:-----------|:-----------------|
|Panel							|pnl		 |pnlGroup		  	|Label							|lbl		 |lblHelpMessage	|
|Check box						|chk		 |chkReadOnly		|List box						|ls			 |lsPolicyCodes		|
|Combo box, drop-down list box	|cbo		 |cboEnglish		|ListView						|lvw		 |lvwHeadings		|
|Button							|btn		 |btnExit		  	|Menu							|mnu		 |mnuFileOpen		|
|Dialog							|dlg		 |dlgFileOpen		|Picture box					|pic		 |picVGA		  	|
|DataTable						|dt			 |dtBiblio		  	|ProgressBar					|prg		 |prgLoadFile		|
|Data-bound combo box			|cbo		 |cboLanguage		|RichTextBox					|rtf		 |rtfReport		  	|
|Data-bound grid				|grid		 |gridQueryResult	|StatusBar						|stt		 |sttDateTime		|
|Data-bound list box			|lst		 |lstJobType		|TextBox						|txt		 |txtLastName		|
|DateTimePicker					|date		 |datePublished		|Timer							|tm			 |tmAlarm		  	|
|Form							|frm		 |frmEntry		  	|Toolbar						|tb			 |tbActions		  	|
|DataGridView					|grid		 |gridPrices		|TreeView						|tv			 |tvOrganization	|
|Horizontal scroll bar			|hb			 |hbVolume		  	|NumericUpDown					|num		 |numMonth		  	|
|Image							|img		 |imgIcon		  	|Vertical scroll bar			|vb			 |vbRate		  	|
|ImageList						|imgls		 |imglsAllIcons		|DataSource						|ds			 |dsSql		  		|
|HyperLink						|lnk		 |lnkHome		  	|                               |			 |					|

#### Sử dụng tiền tố 'I' với các class là interface
```csharp
public interface ICaculator
{
  //...
}
```

#### Nên sử dụng tiền tố 'Is' cho tên biên, method có kiểu dữ liệu là bool
```csharp
  public bool IsEvenNumber(int number)
  {
	bool isValid;
	//...
  }
```

#### Sử dụng tiền tố '_' đối với thuộc tính private
```csharp
private DateTime _registrationDate;
```

## 5. Hậu tố
#### Sử dụng hậu tố với tên các event handler.
```csharp 
public class BarcodeReadEventArgs : System.EventArgs
{
}
```

#### Sử dụng hậu tố với các lớp thuộc tầng kiến trúc(Tier Architecture)
Như: Controller, ViewModel, Attribute, Service, Facade
```csharp 
public class HomeController : Controller
{
}
```

## 6.Viết tắt(Abbreviations)
- Tránh sử dụng Viết tắt. 
- Chỉ sử dụng trong trường hợp tên đầy đủ quá dài.
- Khi viết tắt tránh dài hơn 5 kí tự.
- Từ viết tắt phải phổ biến và sử dụng rộng rãi.
- Ngoại lệ: các chữ viết tắt thường được sử dụng làm tên, chẳng hạn như Id,uid, Xml, Ftp, Uri.
```csharp    
CustomerId customerId;
XmlDocument xmlDocument;
FtpHelper ftpHelper;
UriPart uriPart;
```

## 7. Kí tự cho phép (Char Mask)
Có thể xem thêm ở phần [Khái quát chung](Naming.md#khái-quát-chung)

## 8. Dấu gạch dưới(Underscores )
- Không sử dụng dấu gạch dưới đối với các định danh(identifiers)
- Trừ trường hợp đối với thuộc tính private của class và hằng số