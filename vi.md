

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
