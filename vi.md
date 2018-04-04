## Những lỗi phổ biến nào mà các nhà phát triển ứng dụng tạo ra khi thiết kế database ?

_nguồn https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ số thích hợp**

Đây là điều khá đơn giản nhưng nó vẫn xảy ra mọi lúc. Các khóa ngoại nên có các indexs. Nếu bạn đang sử dụng một trường trong một lệnh WHERE thì bạn nên (hoặc có thể) có một index trong câu lệnh này. Các chỉ mục như vậy thường bao gồm nhiều cột dựa trên các truy vấn bạn cần thực hiện

**2. Không thực thi toàn vẹn tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi, nhưng nếu cơ sở dữ liệu của bạn hỗ trợ tham chiếu toàn vẹn- có nghĩa là tất cả các khóa ngoại phải được đảm bảo luôn tham chiếu đến một thưc thể nào đó đã tồn tại--bạn nên dùng nó.

Điều này được thấy khá phổ biến trong cơ sở dữ liệu MySql. Tôi không tin là MyISAM hỗ trợ nó. Bạn sẽ không thể biết được ai đang sử dụng MyISAM hay InnoDB.

 Xem thêm tại đây:

- [How important are constraints like NOT NULL and FOREIGN KEY if I’ll always control my database input with php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Are foreign keys really necessary in a database design?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Are foreign keys really necessary in a database design?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng các khóa chính thay mặt thay vì các khóa chính tự nhiên **

Các natural key là những khóa trên cơ sở các dữ liệu duy nhất có nghĩa bên ngoài. Các ví dụ điển hình là các mã sản phẩm, 2 chữ cái mã tiểu bang (US), mã số bảo mật mạng xã hội, vân vân,...Các khóa chính kỹ thuật và các khóa chính tự nhiên hoàn toàn không có ý nghĩa bên ngoài hệ thống của ta. Chúng được tạo ra hoàn toàn để xác định thực thể và thông thường các trường (trong SQL Server, MySQL, ...) hoặc chuỗi (trong Oracle) tự động tăng giá trị  hoặc các trình tự (nhất là Oracle).

Theo ý kiến của tôi thì bạn nên **luôn luôn ** sử dụng các khóa kỹ thuật, vấn đề này được đề ra trong những câu hỏi dưới đây:

- [How do you like your primary keys?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [What's the best practice for primary keys in tables?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Which format of primary key would you use in this situation.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Surrogate vs. natural/business keys](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Should I have a dedicated primary key field?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi mà sẽ không nhận được sự đồng tình của tất cả mọi người. Trong khi bạn có thể thấy có người nghĩ là sử dụng các khóa tự nhiên trong một vài trường hợp là OK nhưng bạn sẽ không tìm thấy bất kỳ ý kiến nào cho rằng các khóa tự nhiên là không cần thiết. Nếu bạn hỏi tôi thì đó là một nhược điểm khá nhỏ.

Hãy nhớ rằng các quốc gia cũng có thể không tồn tại (ví dụ Yugoslavia).

**4. Viết các truy vấn yêu cầu

DISTINCT để thực hiện công việc**

Bạn thường gặp nó trong các truy vấn được tạo bởi ORM. Hãy xem phần log của Hibernate và bạn sẽ thấy tất cả các truy vấn đều bắt đầu với

SELECT DISTINCT ..

Điều này ngắn gọn là để đảm bảo bạn không lấy ra các bản ghi giống nhau dẫn tới việc lấy ra các đối tượng bị sao chép. Đôi khi bạn sẽ thấy mọi người đang làm điều này tất tốt. Nếu bạn xem nó quá nhiều thì nó là một lá cờ đỏ thực sự. Điều đó không có nghĩa là DISTINCT là không tốt hay không có các ứng dụng hợp lệ. Nó đều bao gồm cả 2 mặt nhứng không phải là câu lệnh 

Lấy từ [Why I Hate DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):
> Lúc mà có nhiều thứ trở nên không đúng là lúc mà 1 nhà phát triển đang xây dựng các truy vấn lồng nhau, join các bảng với nhau, và đột nhiên ông nhận ra rằng có vẻ như ông đang nhận được các bản ghi lặp lại (hoặc thậm chí nhiều hơn) và phản ứng ngay lập tức ... "giải pháp" của ông ta đối với "vấn đề" này là ném vào truy vấn từ khóa DISTINCT và POOF,  tất cả các rắc rối của ông biến mất.

**5. Sử dụng các lệnh kết hợp lại thay vì lệnh join**

Một sai lầm phổ biến khác của các nhà phát triển ứng dụng cơ sở dữ liệu là không nhận ra các lệnh kết hợp với nhau sẽ tiêu tốn hơn so với các lệnh join (ví dụ Mệnh đề GROUP BY) khi đem so sánh với nhau.

Để cho bạn thấy được vấn đề này phổ biến như nào, tôi đã từng đề cập vấn đề này một vài lần trong các bài viết dưới đây và bị downvoted rất nhiều. Ví dụ:

Lấy từ [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Truy vấn thứ nhất:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Thời gian thực hiện: 0.312 s

> Truy vấn thứ 2:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Thời gian thực hiện: 0.016 s

> Đã rõ. Trong câu lệnh join tôi đã chỉ ra là nó **nhanh hơn 20 lần so với câu câu lệnh sử dụng các lệnh kết hợp.**

**6. Không đơn giản hóa các truy vấn phức tạp thông qua views**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ views nhưng đối với những người làm, họ có thể rất đơn giản hóa các truy vấn nếu được sử dụng một cách khéo léo. Ví dụ, trong một dự án tôi đã sử dụng một mô hình [generic Party](http://www.tdan.com/view-articles/5014/) cho CRM. Đây là một kỹ thuật mô hình rất mạnh và linh hoạt nhưng có thể cần sử dụng đến nhiều lệnh join. Trong mô hình này gồm có:

- **Party**: con người và các tổ chức;
- **Party Role**: những điều mà các bên đã làm, ví dụ như Employee và Employer;
- **Party Role Relationship**: những chức vụ này có liên quan với nhau như thế nào.

Ví dụ:

- Ted là một Person, và là kiểu dữ liệu con của một Party;
- Ted có nhiều roles, trong đó có Employee;
- Intel là một organisation, là kiểu dữ liệu con của một Party;
- Intel có nhiều roles, trong đó có Employer;
- Intel thuê Ted, nghĩa là có một quan hệ giữa các role của 2 bên.

Vì vậy sẽ có 5 bảng để được join lại để liên kết Ted với employer tương ứng. Bạn giả sử tất cả các employees là People (không phải là organasation)và đưa ra view trợ giúp sau:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và đột nhiên bạn có một cái nhìn rất đơn giản về dữ liệu mà bạn muốn nhưng trên một mô hình dữ liệu có tính linh hoạt cao.

**7. Không lọc đầu vào**

Đây là một lỗi lớn. Bây giờ tôi thích PHP nhưng nếu bạn không biết bạn đang làm gì thì thật sự dễ để tạo các trang web dễ bị tấn công. Việc tổng hợp lỗi này được thông kế đầy đủ trong [story of little Bobby Tables](http://xkcd.com/327/).

Dữ liệu cấp bởi người dùng thông qua URL, form và **cookie** nên sẽ luôn luôn bị xem là dễ bị tấn công và lọc nhất. Đảm bảo bạn đang nhận được những gì bạn muốn.

**8. Không sử dụng các câu lệnh được lấy sẵn trước đó**

Các câu lệnh đã được lấy sẵn trước đó là khi bạn biên dịch 1 câu lệnh, trừ các lệnh insert, update và các mệnh để where và các thành phần theo sau. Ví dụ 

SELECT * FROM users WHERE username = 'bob'

với

SELECT * FROM users WHERE username = ?

hoặc

SELECT * FROM users WHERE username = :username

phụ thuộc vào nền tảng của bạn.

Tôi đã nhìn thấy cơ sở dữ liệu mang đến đầu gối của họ bằng cách làm điều này . Về cơ bản, mỗi khi cơ sở dữ liệu hiện đại gặp một truy vấn mới, nó phải biên dịch nó. Nếu nó gặp một truy vấn nó được nhìn thấy trước, bạn đang tạo cho cơ sở dữ liệu có thể cache truy vấn đã biên dịch và dự định có thể thực thi. Bằng cách thực hiện các truy vấn `nhieeuf` nhiều bạn đang giúp cơ sở dữ liệu có thể tính toán và tối ưu hóa cho phù hợp (ví dụ, bằng cách ghim các truy vấn đã được biên dịch trong bộ nhớ) 

Sử dụng các câu lệnh được lấy sẵn cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa về tần suất các truy vấn được sử dụng.

Các lệnh được lấy sẵn cũng sẽ bảo vệ khỏi các tấn công SQL injection tốt hơn
**9. Chuẩn hóa yếu**

Chuẩn hóa cơ sở dữ liệu về cơ bản là quá trình tối ưu hóa thiết kế cơ sở dữ liệu hay là cách bạn tổ chức dữ liệu thành các bảng

Chỉ là trong tuần này tôi đã xem qua một vài đoạn code mà ai đó đã thực hiện gộp một mảng lại và chèn mảng đó vào một cột riêng lẻ trong database. Như thông thường thì ta sẽ coi như mỗi phần từ trong mảng đó tương ứng với một hàng trong 1 bảng con (ví dụ: quan hệ 1-nhiều).

Điều này cũng được đề cập đến trong [Cách tốt nhất để lưu danh sách các danh sách ID của người dùng](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã xem trong những hệ thống khác thì danh sách id này được lưu thành một mảng PHP tuần tự.
> I've seen in other systems that the list is stored in a serialized PHP array.

Tuy nhiên, việc thiếu chuẩn hóa cũng tuân theo nhiều nguyên tắc.
But lack of normalization comes in many forms.

Xem thêm:

- [Chuẩn hóa: Bao nhiêu là đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Tại sao bạn cần chuẩn hóa cơ sở dữ liệu? ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hóa dư thừa**

Điều này dường như có vẻ mâu thuẫn với phần trước nhưng việc chuẩn hóa cũng giống như nhiều thứ khác, nó chỉ là một công cụ mà thôi.  Nó chỉ là một phương tiện để đạt được mục đích của ta chứ không phải bất kỳ điều gì khác. Tôi nghĩ là rất nhiều nhà phát triển đã quên điều này và bắt đầu coi một "công cụ" là một "kết thúc". Unit test là một ví dụ điển hình cho điều vừa nói. 

Tôi đã từng làm việc trên một hệ thống có mô hình phân cấp khách hàng rất đồ sộ như sau: 

Người được cấp phép -&gt;  Nhóm đại lý -&gt; Công ty -&gt; Khách hàng -&gt; ...

Nhu vậy là bản phải join 11 bảng lại với nhau để có thể lấy dữ liệu có đầy đủ nội dung. Đó là một ví dụ điển hình về quá trình chuẩn hóa quá mức.

Bổ sung thêm nữa, kỹ thuật chuẩn hóa ngược có thể đem lại những lợi ích rất lớn nhưng bạn phải thực sự cẩn thẩn khi làm điều này.

Xem thêm:

- [Tại sao Việc chuẩn hóa cơ sở dữ liệu quá mức lại có thể là một điều không tốt?](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Chuẩn hóa cơ sở dữ liệu tới mức nào là đủ?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Những lúc không cần chuẩn khóa cơ sở dữ liệu sql](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Đôi khi chuẩn hoá không tuân theo thường lệ](http://www.codinghorror.com/blog/archives/001152.html)
- [Toàn tập chuẩn hóa cơ sở dữ liệu được tranh luận trong Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Sử dụng các vòng cung duy nhất**

Một vòng cung độc quyền là một lỗi phổ biến khi mà một bảng được tạo ra có hai hoặc nhiều hơn 2 khóa ngoại nhưng chỉ có một trong số các khóa ngoại đó có thể đươc thiết lập non-null. Đây là một lỗi lớn. Điều khiển trở nên khó khăn hơn nhiều trong việc duy trì tính toàn vẹn dữ liệu. Nếu như thế, thậm trí toàn vẹn ràng buộc cũng không được duy trì, không có bất kỳ thứ gì ngăn cản các khóa ngoại đó được thiết lập (mặc dù gây khó khăn hơn trong việc kiểm tra các ràng buộc).

Trích từ bài viết [Hướng dẫn thiết kế cơ sở dữ liệu quan hệ trong thực tiễn](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng tôi khuyến cáo không nên tạo exclusive arc bất cứ khi nào, lý do là nó có thể gấy lúng túng khi lập trình và những khó khăn trong việc bảo trì.

**12. Không phân tích hiệu năng trong tất cả các câu truy vấn**

Tính thực dụng được ưu tiên hàng đầu, đặc biệt trong cơ sở dữ liệu. Nếu bạn đang khăng khăng tuân theo các nguyên tắc đã trở thành giáo điều thì bạn có lẽ đã mắc phải sai lầm. Ví dụ chính là các truy vấn gom nhóm ở phần trên. Cách truy vấn gom nhóm trong có vẻ 'ngắn gon' nhưng hiệu năng lại không tốt. Việc so sánh hiệu năng nên kết thúc cuộc tranh luận (nhưng không), mà còn nhiều hơn thế nữa: việc đưa ra view thống báo không tốt trong phần đầu là ngu dốt, thậm chí là nguy hiểm. 

**13. Quá phụ thuộc vào UNION ALL và đặc biệt cấu trúc UNION**

Một UNION trong các thuật ngữ SQL đơn giản chỉ là ghép tập dữ liệu đồng nhất lại với nhau, có nghĩa là chúng có cùng loại và số cột. Sự khác biệt giữa chúng là UNION ALL là một sự ghép nối đơn giản và nên được ưu tiên hơn bất cứ khi nào có thể, trong khi một UNION ngầm định sẽ thực thi một lệnh  DISTINCT để loại bỏ các bản sao trùng lặp.

UNIONs, như DISTINCT, có vị trí của nó. Có các ứng dụng hợp lệ. Nhưng nếu bạn thấy mình đang làm rất nhiều, đặc biệt trong các truy vấn phụ, thì có thể bạn đang làm sai. Điều đó có thể phản ảnh việc truy vấn kém hoặc mô hình dữ liệu được thiết kế kém, buộc bạn phải làm những việc như vậy.

UNION, đặc biệt khi sử dụng trong các lệnh join hoặc các truy vấn phụ phụ thuộc, có thể làm hỏng cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dụng các lệnh điều kiến OR trong truy vấn**

Điều này có vẻ như vô hại. Nhưng cho cùng thì phép AND vẫn là ok nhất. Liệu phép OR cũng ổn chứ? Không hề. Về cơ bản lệnh điều kiện AND **thu hẹp** tập dữ liệu, trong khi lệnh điều kiện OR lại **mở rộng** nhưng không phải theo cách để tối ưu hoá. Đặc biệt khi các lệnh điều kiện OR khác nhau có thể trùng nhau, do đó buộc bộ tối ưu hóa phải thực hiện một lệnh DISTINCT trong kết quả trả về.

Không tốt:

... WHERE a = 2 OR a = 5 OR a = 11

Tốt:

... WHERE a IN (2, 5, 11)

Now your SQL optimizer may effectively turn the first query into the second. But it might not. Just don't do it.
Bây giờ, bộ tối ưu hoá SQL của bạn có thể chuyển câu lệnh truy vấn đầu tiên thành câu lệnh truy vấn thứ hai. Nhưng nó cũng có thể không. Nhưng không nên như vậy.

**15. Không thiết kế mô hình dữ liệu của mình đi kèm theo các giải pháp hiệu suất cao**

Đây là một điều khó để có thể định lượng. Nó thường được đánh giá bởi khả năng thực hiện. Nếu bạn thấy mình đang viết các truy vấn nhỏ cho các tác vụ tương đối đơn giản hoặc các truy vấn để tìm các thông tin tương đối đơn giản nhưng không có thể thực hiện thì có lẽ bạn có một mô hình dữ liệu nghèo nàn.

Trong phần này đã tóm tắt tất cả những phần trước đó, nhưng điều đáng nói là những công việc như tối ưu hóa truy vấn thường được thực hiện trước tiên trong khi đáng ra nó nên được thực hiện thứ hai. Trước hết bạn phải đảm bảo bạn có một mô hình dữ liệu tốt trước khi cố gắng tối ưu hóa hiệu năng. Như Knuth đã nói:
> Việc tối ưu ngay từ đầu là nguyên nhân của tất cả những sai lầm.

**16. Sử dụng các giao dịch cơ sở dữ liệu không chính xác**

Tất cả các dữ liệu thay đổi cho một quá trình cụ thể phải là nguyên tử. Ví dụ nếu thực thi thành công, nó sẽ thưc hiện toàn bộ. Nếu thực thi không thành công thì dữ liệu không bị thay đổi. Không nên làm thay đổi nửa vời.

Cách đơn giản nhất để đạt được điều này là toàn bộ thiết kế hệ thống nên cố gắng hỗ trợ tất cả các thay đổi dữ liệu thông qua các câu lệnh INSERT / UPDATE / DELETE đơn lẻ. Trong trường hợp này, không cần có bất kỳ xử lý Transaction đặc biệt nào, vì các Engine trong cơ sở dữ liệu của bạn tự động làm như vậy. 

Tuy nhiên, nếu bất kỳ quy trình nào yêu cầu nhiều lệnh được thực hiện như là một đơn vị để đảm bảo dữ liệu ở trạng thái nhất quán, thì Transaction Control là cần thiết: 

- Bắt đầu Transaction trước câu lệnh đầu tiên.
- Commit Transaction sau câu lệnh cuối cùng.
- Nếu bất kỳ lỗi gì xảy ra thì Rollback Transaction. Và không được quên bỏ qua/hủy tất cả các câu lệnh viết phía sau phần lệnh xảy ra lỗi.

Cũng nên chú ý đến cách mà lớp cở dữ liệu và các engine tương tác với nhau như thế nào.

**17. Không hiểu mô hình "dựa trên cơ sở**

Ngôn ngữ SQL tuân theo một mô hình cụ thể phù hợp với từng loại vấn đề cụ thể. Các nhiều extension khác nhau của nhà cung cấp cụ thể tuy nhiên, các ngôn ngữ phải giải quyết các vấn đề thường gặp trong ngôn ngữ như Java, C #, Delphi,...

Sự thiếu hiểu biết này thể hiện theo một vài cách.

- Áp dụng không thích hợp quá nhiều quy tắc hoặc bắt buộc logic nên cơ sở dữ liệu.
- Việc sử dụng không phù hợp hoặc lạm dụng các con trỏ. Đặc biệt là khi chỉ cần một truy vấn duy nhất là đủ
- Hiểu sai rằng trigger được thực hiện ngay khi mỗi dòng bị ảnh hưởng trong cập nhiều dòng

Xác định phân chia trách nhiệm rõ ràng và cố gắng sử dụng công cụ thích hợp để giải quyết từng vấn đề.

>Denormalization là kỹ thuật trong nhóm kỹ thuật tối ưu thiết kế cơ sở dữ liệu quan hệ (RDBMS)
