# Spark-Properties-RDD-and-DataFrame
Study about Spark properties, Spark RDD and Spark DataFrame

## Phần 1: Spark Propreties
### I. Đôi nét về Spark Properties
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Thuộc tính Spark – Spark Properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng cho từng ứng dụng. Các thuộc tính này có thể được cài đặt trực tiếp trên SparkConf được chuyển đến SparkContext của bạn. SparkConf cho phép định cấu hình một số thuộc tính chung (ví dụ: URL chính và tên ứng dụng), cũng như các cặp key-value thông qua phương thức set().</p>
  
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em><b>Ví dụ</b></em>: Khởi tạo một ứng dụng với 2 luồng:</p>
  
```python
      val conf = new SparkConf()
                 .setMaster("local[2]")
                 .setAppName("CountingSheep")
      val sc = new SparkContext(conf)
 ```
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Trong đó, local[2] cho biết tối thiểu có 2 luồng đang chạy song song, giúp phát hiện lỗi chỉ tồn tại khi chạy trong bối cảnh phân tán.</br>
                    &nbsp;&nbsp;&nbsp;&nbsp; Các thuộc tính chỉ định một số khoảng thời gian với một đơn vị thời gian. Các định dạng sau được Spark chấp nhận:</p>

```note
      25ms (milliseconds)          3h (hours)   
      5s (seconds)                 5d (days)
      10m or 10min (minutes)       1y (years)
```

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các định dạng thuộc tính kích thước byte có trong Spark” </p>

```note
      1b (bytes)                                    1g or 1gb (gibibytes = 1024 mebibytes)
      1k or 1kb (kibibytes = 1024 bytes)            1t or 1tb (tebibytes = 1024 gibibytes)
      1m or 1mb (mebibytes = 1024 kibibytes)        1p or 1pb (pebibytes = 1024 tebibytes)
```
  
### II. Tải động với Spark Properties ((Dynamically loading Spark Properties)
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Trong một sô trường hợp, ta có thể tránh việc thiết lập cứng cho các cấu hình mặc định trong một SparkConf. </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em><b>Ví dụ</b></em>: Nếu muốn chạy cùng một ứng dụng với các bản gốc khác nhau hoặc số lượng bộ nhớ khác nhau thì chỉ cần dùng <em>SparkConf()</em> mà Spark cung cấp, cho phép tạo một SparkConf trống.</p>

```python
      val sc = new SparkContext(new SparkConf())
```
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Sau đó, chỉ việc cung cấp các giá trị cấu hình trong lúc chạy Spark:</p>

```python
      ./bin/spark-submit --name "My app" --master local[4] --conf spark.eventLog.enabled=false --
      conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails -XX:+PrintGCTimeStamps" myApp.jar
```

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Trong đó, công cụ <em>spark-submit</em> và trình bao Spark hỗ trợ hai cách để tải cấu hình động. Đầu tiên là các tùy chọn dòng lệnh, chẳng hạn như <em>--master</em>, như được hiển thị ở trên. <em>spark-submit</em> có thể chấp nhận bất kỳ thuộc tính Spark nào bằng cách sử dụng <em>--conf/-c</em> cờ, nhưng sử dụng cờ đặc biệt cho các thuộc tính đóng một vai trò trong việc khởi chạy ứng dụng Spark. Đang chạy <em>./bin/spark-submit –help</em> sẽ hiển thị toàn bộ danh sách các tùy chọn này.</p>

### III. Các thuộc tính của Spark
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các thuộc tính của Spark chủ yếu được chia thành hai loại: </p>

<ul align="justify">
  <li><em>Liên quan đến triển khai</em>: Như <b><em>spark.driver.memory, spark.executor.instances</em></b>. Loại thuộc tính này có thể không bị ảnh hưởng khi thiết lập theo chương trình <b>SparkConf</b> trong thời gian chạy hoặc hành vi tùy thuộc vào trình quản lý cụm và chế độ triển khai đã chọn trước. Do đó nên đặt thông qua file cấu trúc hoặc tùy chọn dòng lệnh <b><em>spark-submit</em></b>.</li></br>
  <li><em>	Liên quan đến kiểm soát thời gian chạy Spark</em>: Như <b><em>spark.task.maxFailures.</em></b>.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Apache Spark cung cấp môt bộ giao diện người dùng trẻn website: http://localhost:4040 (Job, Stages, Tasks, Strorage, Environment, Executors và SQL). Để có thể xem các thược tính của Spark, mọi người vào thẻ Environment. Ngoài  ra, có thể xác định giá trị mặc định thông qua <em>spark-defaults.conf</em>. Các thuộc tính mặc định có sẵn trong Spark đều có giá trị mặc định hợp lý. </p>

#### 1.	Một vài thuộc tính ứng dụng - Application Properties
<p align="center"><img src ="https://user-images.githubusercontent.com/77878466/106383406-5a1fba00-63f8-11eb-9635-53859153f72d.PNG" width="90%"/></p>

#### 2.	Một vài thuộc tính xáo trộn - Shuffle Behavior
<p align="center"><img src ="https://user-images.githubusercontent.com/77878466/106383427-76235b80-63f8-11eb-92ab-913d38f83f72.PNG" width="90%"/></p>

#### 3.	Giao diện người dùng Spark - Spark UI
<p align="center"><img src ="https://user-images.githubusercontent.com/77878466/106383436-876c6800-63f8-11eb-9d26-4ab6528f1f96.PNG" width="90%"/></p>

#### 4.	Nén và tuần tự hóa (Compression and Serialization)
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em>spark.rdd.compress</em> - Có nén các phân vùng tuần tự</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em><b>Ví dụ</b></em>: StorageLevel.MEMORY_ONLY_SERtrong Java và Scala hoặc StorageLevel.MEMORY_ONLY trong Python). Có thể tiết kiệm không gian đáng kể với chi phí tăng thêm thời gian CPU. Nén sẽ sử dụng tới thuộc tính spark.io.compression.codec. Ngoài ra còn có:</p>

<ul align="justify">
  <li>spark.serializer</li>
  <li>spark.serializer.objectStreamReset</li>
  <li>spark.kryoserializer.buffer</li>
  <li>spark.kryo.registrator</li>
  <li>spark.kryo.referenceTracking, ...</li>
</ul>

### IV. Các thuộc tính khác
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Ngoài các loại thuộc tính trên Spark còn hỗ trợ nhiều loại thuộc tính khác nhau:</p>

<ul align="justify">
  <li>Môi trường thực thi (Runtime Environment)</li>
  <li>Quản lý bộ nhớ (Memory Management)</li>
  <li>Hành vi thực thi (Execution Behavior)</li>
  <li>Chỉ số thực thi (Executor Metrics)</li>
  <li>Kết nối mạng (Networking)</li>
  <li>Lập lịch (Scheduling)</li>
  <li>Chế độ thực thi rào cản (Barrier Execution Mode)</li>
  <li>Phân bố động (Dynamic Allocation)</li>
  <li>Cấu hình Thread (Thread Configurations)</li>
  <li>Bảo mật (Security)</li>
</ul>

## Phần 2: Spark Resilient Distributed Datasets - Spark RDD
### I.	Tổng quát về Resilient Distributed Datasets – RDD
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD (Resilient Distributed Datasets) được định nghĩa trong Spark Core. Nó đại diện cho một collection các item đã được phân tán trên các cluster, và có thể xử lý phân tán. PySpark sử dụng PySpark RDDs và nó chỉ là 1 object của Python nên khi bạn viết code RDD transformations trên Java thực ra khi run, những transformations đó được ánh xạ lên object PythonRDD trên Java.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Bên cạnh đó, RDD còn được hiểu là cấu trúc dữ liệu nền tảng của Spark, được sử dụng để phát triển Spark từ khi dự án này mới được ra đời. Resilient ở đây có thể hiểu là khả năng khôi phục dữ liệu khi dữ liệu xảy ra lỗi hoặc bị mất dữ liệu trong quá trình sử dụng. Distributed có nghĩa là các phần tử và các đối tượng (objects) trong Spark là không thể thay đổi (immutable) và được phân tán ra nhiều nodes khác nhau trong một cluster. Chính thuộc tính này của RDD cho phép Spark có thể thực hiện các thuật toán và tiến hành xử lý một cách song song, qua đó giúp tăng tốc độ và hiệu suất của hệ thống.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDDs có thể chứa bất kỳ kiểu dữ liệu nào của Python, Java, hoặc đối tượng Scala, bao gồm các kiểu dữ liệu do người dùng định nghĩa. Thông thường, RDD chỉ cho phép đọc, phân mục tập hợp của các bản ghi. RDDs có thể được tạo ra qua điều khiển xác định trên dữ liệu trong bộ nhớ hoặc RDDs, RDD là một tập hợp có khả năng chịu lỗi mỗi thành phần có thể được tính toán song song.</p>

### II.	Các đặc điểm của Spark RDD
#### 1.	Tính toán trong bộ nhớ
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark RDD cung cấp khả năng tính toán trong bộ nhớ. Nó lưu trữ các kết quả trung gian trong bộ nhớ phân tán (RAM) thay vì lưu trữ ổn định (đĩa).</p>

#### 2.	Lazy Evaluations
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Tất cả các phép biến đổi trong Apache Spark đều được gọi là lười biếng (lazy), ở chỗ chúng không tính toán ngay kết quả của chúng. Thay vào đó, nó chỉ nhớ các phép biến đổi được áp dụng cho một số tập dữ liệu cơ sở.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark tính toán các phép biến đổi khi một hành động yêu cầu kết quả cho driver của chương trình.</p>

#### 3.	Khả năng chịu lỗi
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD có khả năng chịu lỗi vì chúng theo dõi thông tin dòng dữ liệu để tự động xây dựng lại dữ liệu bị mất khi bị lỗi. Nó xây dựng lại dữ liệu bị mất khi lỗi bằng cách sử dụng dòng (lineage), mỗi RDD nhớ cách nó được tạo ra từ các tập dữ liệu khác (bằng các phép biến đổi như map, join hoặc GroupBy) để tạo lại chính nó.</p>

#### 4.	Tính bất biến
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Dữ liệu an toàn để chia sẻ trên các process. Ngoài ra, nó cũng có thể được tạo hoặc truy xuất bất cứ lúc nào giúp dễ dàng lưu vào bộ nhớ đệm, chia sẻ và nhân rộng. Vì vậy, chúng ta có thể sử dụng nó để đạt được sự thống nhất trong tính toán.</p>

#### 5.	Phân vùng
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Phân vùng là đơn vị cơ bản của tính song song trong Spark RDD. Mỗi phân vùng là một phân chia dữ liệu hợp lý mà có thể thay đổi được. Ta có thể tạo một phân vùng thông qua một số biến đổi trên các phân vùng hiện có.</p>

#### 6.	Sự bền bỉ (Persistence)
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Người dùng có thể cho biết họ sẽ sử dụng lại những RDD nào và chọn hướng lưu trữ cho họ (ví dụ: lưu trữ trong bộ nhớ hoặc trên Đĩa).</p>

#### 7.	Hoạt động chi tiết thô (Coarse-grained Operations)
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Nó áp dụng cho tất cả các phần tử trong bộ dữ liệu thông qua map hoặc fiter hoặc group theo hoạt động.</p>

#### 8.	Vị trí – độ dính (Location – Stickiness)
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD có khả năng xác định ưu tiên vị trí để tính toán các phân vùng. Tùy chọn vị trí đề cập đến thông tin về vị trí của RDD. DAGScheduler đặt các phân vùng theo cách sao cho tác vụ gần với dữ liệu nhất có thể. Do đó, tốc độ tính toán có thể tăng.</p>

### III.	Các hoạt động và cách áp dụng các hoạt động trên RDD
#### 1.	Transformation
#### 2.	Action

### IV.	Một số code minh họa các hoạt động
#### 1.	Count()
#### 2.	Collect()
#### 3.	foreach(f)
#### 4.	filler(f)
#### 5.	map(f, securePartitioning = False)
#### 6.	reduce(f)
#### 7.	join(other, numPartitions = none)
#### 8.	cache()

## Phần 3:	Spark DataFrame
### I.	Tổng quát về Spark DataFrame

### II. Lợi ích mà DataFrame mang lại

### III.	Các tính năng của DataFrame và nguồn dữ liệu PySpark
#### 1.	Các tính năng
#### 2. Nguồn dữ liệu PySpark
