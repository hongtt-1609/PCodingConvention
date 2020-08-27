# DotNet Coding Convertion

Phần này bao gồm một tập hợp các quy tắc coding trong dự án DotNet

## Khái quát chung
- Tuân thủ [Quy ước đặt tên](./Naming.md)

## 1. Quy ước về bố cục (Layout Conventions)
#### Quy ước chung
- Thụt lề 4 kí tự.
- Giữ khoảng trắng(space) trước dấu mở ngoặc hoặc các toán tử(operators: +,-,==,&&,||,...).
- Không viết 2 lệnh trên 1 dòng.
- Viết mỗi khai báo trên mỗi dòng.
- Thêm ít nhất một dòng trống giữa các method, thuộc tính.
- Dấu mở ngoặc "{" được đặt trên 1 dòng mới.
- Function có không quá 7 params, nếu quá thì phải tạo model
- Phân thân function không nên dài quá 20 dòng.
- Số lượng chữ mỗi dòng nên hạn chế dưới 80 kí tự và tối đa 130 kí tự.
- File source code không nên quá dài(không nên vượt quá 2000 LOC (Lines of Code)).
- Nên thực hiện format lại code bằng tool hỗ trợ. (trên vs sử dụng cú pháp Ctrl + K + D)

#### Phải khải báo quyền truy cập rõ ràng(access modifier)
```csharp
private int Sum(int a, int b) 
{
	//...
}
```

#### Trình tự khai báo các field, properties, constructors: 
Private -> Protected -> Internal -> public

#### Phân nhóm #region đối với class có nhiều dòng
Ví dụ phân nhóm một class
- Declaration
- Property
- Constructor
- Method/Function
- Event

#### Giữ các thành phần trong class theo một thứ tự nhất định
1. các thuộc tính private
2. các thuộc tính constants
3. các trường thuộc tính statics
4. các contructor khởi tạo, hoặc các hàm init
5. các events
6. các thuộc tính public
7. các method (có thể phân nhóm theo quyền truy cập hoặc nhóm chức năng)

## 2. Quy ước về comment (Commenting Conventions)
- Comment phải được đặt trên 1 dòng riêng biệt
- Có khoảng trắng giữa dấu phân cách comment (//) và text
- Có thể sử dụng cách dưới đây hoặc tùy vào từng dự án để đưa ra quy ước comment cho hợp lý.

#### sử dụng comment dài đối trước tên class, tên method
Quy tắc viết comment
```csharp
/// <summary>
/// Name: [Tên ngắn gọn của đối tượng]
/// Func: [Nội dung xử lý]
/// Created: [Ngày tháng yyyy/mm/dd] [người tạo]
/// Update : [Ngày tháng yyyy/mm/dd] [người update] - [Nội dung thay đổi]
/// </summary>
/// <param name="[tên param]">[dữ liệu của param]</param>
/// <returns>[Dữ liệu trả về]</returns>
```

Ví dụ về comment class
```csharp
/// <summary>
/// Name: Caculator Emulator
/// Func: Create Basic Calculator
/// Created: 2020/08/24 dongvd
/// </summary>
public class Caculator
```

Ví dụ về comment method
```csharp
    /// <summary>
    /// Name: Caculator Sum
    /// Func: Caculator sum of arr string params
    /// Created: 2020/08/24 dongvd
    /// Update : 2020/10/24 dongvd - update input params from int[] to string[]
    /// </summary>
    /// <param name="args">input params</param>
    /// <returns>sum of params</returns>
    public int Sum(params string[] args)
```

#### Sử dụng comment ngắn khi cần chú thích logic xử lý, chú thích field
```csharp
    public int Sum(params string[] args)
    {
        //B01: convert string[] to int[]
        int[] myInts = Array.ConvertAll(args, int.Parse);

        return myInts.Sum();
    }
```

## 3. Quy ước code (Language Guidelines)

#### 3.1 Quy ước về nối chuỗi
Sử dụng nội suy chuỗi khi cộng chuỗi ngắn
```csharp
	string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```
Hoặc sử dụng String.Format()  
```csharp
	string displayName = String.Format("{0}, {1}", nameList[n].LastName, nameList[n].FirstName);
```

Sử dụng StringBuilder nếu phải xử lý nối chuỗi dài
```csharp
	var manyPhrases = new StringBuilder();
	for (var i = 0; i < 10000; i++)
	{
		manyPhrases.Append(phrase);
	}
```

#### 3.2 Quy ước về câu lệnh truy vấn linq
Căn chỉnh các mệnh đề truy vấn dưới mệnh đề from
```csharp
    var seattleCustomers2 = from customer in customers
                            where customer.City == "Seattle"
                            orderby customer.Name
                            select customer;
```
Hoặc để trên 1 dòng nếu ngắn gọn
```csharp
    var query = from product in products where product.Price > 10 select product;
```

#### 3.3 Không sử dụng các giá trị chữ, số hoặc chuỗi trong mã. hãy khai báo nó bằng một hằng số
```csharp
public class Whatever  
{
    public static readonly Color PapayaWhip = new Color(0xFFEFD5);
    public const int MaxNumberOfWheels = 18;
    public const byte ReadCreateOverwriteMask = 0b0010_1100;
}
```

#### 3.4 Quy ước về biến
- Sử dụng kiểu dữ liệu C# thay vì .Net.
- Cố gắng khởi tạo giá trị khi khai báo biến.
- Luôn cố gắng chọn kiểu dữ liệu đơn giản và tối ưu nhất.
- Sử dụng kiểu dữ liệu cho giới hạn phù hợp.
- Tránh sử dụng sbyte, short, unit, ulong.
- Tránh khai báo các biến có giá trị khởi tạo là chuỗi kí tự. Nên sử dụng hằng số, resource, config cho trường hợp này.
- Tránh truy cập trực tiếp vào các phần tử của đối tượng. Trước khi truy cập phải check null.

#### 3.5. Các khối mã lồng nhau không nên quá 4 bậc
ví dụ về việc xử lý lồng nhau quá nhiều cấp
```csharp
if (st != null)
{
    if (frameIndex < st.FrameCount)
    {
        StackFrame frame = st.GetFrame(frameIndex);
        if (frame != null)
        {
            MethodBase method = frame.GetMethod();
            if (method != null)
            {
                collection["method"] = method.Name;
                if (method.DeclaringType != null)
                {
                    collection["class"] = method.DeclaringType.FullName;
                }
            }
            collection["lineNum"] = frame.GetFileLineNumber().ToString();
        }
    }
}
```

Nên chuyển sang cách xử lý dưới đây
```csharp
if (st == null)
    return collection;

if (frameIndex > st.FrameCount)
    return collection;

StackFrame frame = st.GetFrame(frameIndex);

if (frame == null)
    return collection;

MethodBase method = frame.GetMethod();
if (method == null)
    return collection;

collection["method"] = method.Name;

if (method.DeclaringType == null)
    return collection;

collection["class"] = method.DeclaringType.FullName;
collection["lineNum"] = frame.GetFileLineNumber().ToString();
return collection;
```

#### 3.6 Quy ước về điều kiện trong câu lệnh if
Tránh viết điều kiện trong khối lệnh if phực tạp. Hãy đơn giản nó bằng các tạo các function hoăc các biến.
```csharp
if (((value > _highScore) && (value != _highScore)) && (value < _maxScore))
{…} 
```

nên đổi thành
```csharp
isHighScore = (value >= _highScore);
isTiedHigh = (value == _highScore);
isValid = (value < _maxValue);
if ((isHighScore && ! isTiedHigh) && isValid)
{…}
```

#### 3.7 Quy ước về sử dụng các câu lệnh rẽ nhánh.
Tránh sử dụng quá nhiều câu lệnh rẽ nhánh trong một function. Nó sẽ làm code của bạn khó hiểu hơn.
Có thể tách thành các model giúp code có thể dễ dàng bảo trì và mở rộng.

#### 3.8 Quy ước về xử lý Exceptions
- Không dùng khối lệnh try/catch để điều khiển luồng dữ liệu.
- Nên thực hiện xử lý kiểm tra dữ liệu thay vì để xảy ra ngoại lệ.
- Không khai báo các đoạn catch là rỗng. Nó có thể làm bạn không thể kiểm soát được lỗi.
- Tránh sử dụng try/catch lồng nhau.
- Thứ tự các bộ lọc ngoại lệ nên sắp xếp theo tần suất xuất hiện của nó.
- Tránh tạo lại các ngoại lệ đã được định nghĩa trong hệ thống.
- Nên có xử lý ngoại lệ ở tầng cao nhất của mỗi Library.

#### 3.9 Quy ước về Log
- Ghi log đối với tất cả các câu lệnh truy vấn đến cơ sở dữ liệu.
- Ghi log đối với tất cả thao tác người dùng.
- Ghi log với tất cả exception














