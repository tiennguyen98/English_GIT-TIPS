#10 Lới khuyên giúp nâng kỹ năng Git của bạn lên tầm cao mới

Gần đây, chúng tôi đã xuất bản một vài hướng dẫn để giúp bạn làm quen với Git cơ bản và sử dụng Git trong môi trường làm việc nhóm. Các lệnh mà chúng ta đã thảo luận đủ để giúp một nhà phát triển tồn tại trong thế giới Git. Trong bài đăng này, chúng tôi sẽ cố gắng tìm hiểu cách quản lý thời gian một cách hiệu quả và tận dụng các tính năng mà Git cung cấp..

Lưu ý: Một số lệnh trong bài viết này bao gồm một phần của lệnh trong dấu ngoặc vuông (ví dụ: git add -p [file_name]). Trong những ví dụ này, bạn sẽ chèn thêm số cần thiết,định danh, vân vân mà không có dấu ngoặc vuông.

##1. Tự động hoàn thành lệnh Git

Nếu bạn chạy lệnh Git thông qua dòng lệnh, đó là một công việc mệt mỏi mỗi lần gõ các lệnh bằng tay. Để giúp đỡ việc này, bạn có thể tự động hoàn thành lệnh Git trong vòng vài phút.

Để có được script, hãy chạy lệnh sau trong hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp theo, thêm các dòng sau vào file ```~ / .bash_profile``` của bạn:

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập đến điều này trước đó, tôi không thể nhấn mạnh đủ: Nếu bạn muốn sử dụng đầy đủ các tính năng của Git, bạn chắc chắn nên chuyển sang giao diện dòng lệnh!

##2. Loại bỏ file trong Git

Bạn có mệt mỏi với các file biên dịch (như ```.pyc```) xuất hiện trong kho Git của bạn không? Hay bạn quá chán khi chúng thêm vào Git? Không cần nhìn đâu xa, có một cách mà bạn có thể báo với Git để bỏ qua hoàn toàn một số file và thư mục. Đơn giản chỉ cần tạo một file với tên ```.gitignore``` và liệt kê các file và thư mục mà bạn không muốn Git theo dõi. Bạn có thể thực hiện ngoại lệ bằng cách sử dụng dấu chấm than (!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3. Ai làm lỗi code của tôi?

Đó là bản năng tự nhiên của con người để đổ lỗi cho người khác khi có điều gì đó không ổn. Nếu máy chủ sản xuất của bạn bị hỏng, rất dễ để tìm ra thủ phạm - chỉ cần thực hiện một ```git blame```. Lệnh này cho bạn thấy tác giả của mỗi dòng trong một file, commit thay đổi cuối cùng trong dòng đó và mốc thời gian của commit.

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

Và trong ảnh chụp màn hình bên dưới, bạn có thể thấy lệnh này sẽ trông như thế nào trên một repository lớn hơn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Xem lại lịch sử của Repository

Chúng tôi đã có một cái nhìn về việc sử dụng ```git log``` trong một hướng dẫn trước đó, tuy nhiên, có ba lựa chọn mà bạn nên biết.

* ```--oneline``` – Nén thông tin hiển thị bên cạnh mỗi commit để giảm bớt commit và commit message, tất cả được hiển thị trong một dòng.
* ```--graph``` – Lựa chọn này cho ra một biểu diễn đồ họa dựa trên văn bản của lịch sử ở phía bên tay trái của đầu ra. Không sử dụng nếu bạn đang xem lịch sử cho một nhánh.
* ```--all``` – Cho thấy lịch sử của tất cả các nhánh.

Dưới đây là kết quả khi kết hợp các lựa chọn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. Không bao giờ để mất theo dõi của một commit

Hãy nói rằng bạn đã commit một điều gì đó mà bạn không muốn và kết thúc bằng cách thực hiện hard reset để trở lại trạng thái trước của bạn. Sau đó, bạn nhận ra rằng bạn đã mất một số thông tin khác trong tiến trình và muốn khôi phục lại hoặc ít nhất là xem nó. Đây là lúc ```git reflog``` có thể giúp đỡ bạn.

Một ```git log``` đơn giản cho bạn thấy commit mới nhất, cha của nó, cha của cha nó, vân vân. Tuy nhiên, ```git reflog``` là một danh sách các commit mà head trỏ đến. Hãy nhớ rằng nó là local trong hệ thống của bạn; nó không phải là một phần trong repository của bạn và không bao gồm trong push hoặc merge.

Nếu tôi chạy ```git log```, tôi nhận được các commit là một phần của repository của tôi:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, ```git reflog``` cho thấy một commit (```b1b0ee9 - HEAD @ {4} ```) đã bị mất khi tôi thực hiện hard reset:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Phân loại các phần của một file đã thay đổi cho một commit

Nói chung, cách làm tốt là tạo commit dựa trên tính năng, nghĩa là mỗi commit phải đại diện cho một tính năng hoặc sửa lỗi. Hãy xem xét điều gì sẽ xảy ra nếu bạn cố định hai lỗi hoặc thêm nhiều tính năng mà không commit sự thay đổi. Trong trường hợp tình huống như vậy, bạn có thể đặt những thay đổi trong một commit duy nhất. Nhưng có một cách tốt hơn: Stage các file riêng lẻ và commit chúng một cách riêng lẻ.

Giả sử bạn đã thực hiện nhiều thay đổi cho một file và muốn chúng xuất hiện trong các commit riêng biệt. Trong trường hợp đó, chúng ta thêm các file bằng tiền tố ```-p``` vào các lệnh thêm của chúng ta.

```

git add -p [file_name]

```

Chúng ta hãy cùng nhau chứng minh điều đó. Tôi đã thêm ba dòng mới vào ```file_name``` và tôi chỉ muốn dòng đầu tiên và dòng thứ ba xuất hiện trong commit của tôi. Hãy xem những gì ```git diff``` cho chúng ta thấy.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi chúng ta thêm tiền tố ```-p``` vào lệnh```add``` của chúng ta.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Có vẻ như Git cho rằng tất cả những thay đổi đều là một phần của cùng một ý tưởng, do đó nhóm chúng thành một khối lớn. Bạn có các lựa chọn sau:

* Nhập y để stage khối đó
* Nhập n để không stage khối đó
* Nhập e để tự chỉnh sửa các khối 
* Nhập d để thoát hoặc đi tới file tiếp theo.
* Nhập s để chia khối.

Trong trường hợp này của chúng ta, chúng ta chắc chắn muốn chia nhỏ nó thành các phần nhỏ hơn để bổ sung có chọn lọc và bỏ qua phần còn lại.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Như bạn thấy, chúng tôi đã thêm vào dòng đầu tiên và thứ ba và bỏ qua thứ hai. Sau đó bạn có thể xem tình trạng của repository  và thực hiện một commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Squash nhiều commit

Khi bạn gửi code của mình để xem xét và tạo 1 pull request (thường xảy ra trong các dự án mã nguồn mở), bạn có thể được yêu cầu thực hiện thay đổi code của mình trước khi nó được chấp nhận. Bạn thực hiện thay đổi, chỉ để được yêu cầu thay đổi lại nó trong lần xem xét tiếp theo. Trước khi bạn biết nó, bạn có một vài commit thêm. Lý tưởng nhất là bạn có thể sử dụng lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```

Nếu bạn muốn squash hai commit cuối, lệnh bạn chạy là như sau.

```

git rebase -i HEAD~2

```

Khi chạy lệnh này, bạn sẽ được đưa đến một giao diện tương tác liệt kê các commit và sẽ hỏi bạn chọn cái nào để squash. Lý tưởng nhất là bạn ```pick``` các commit mới nhất và```squash``` những cái cũ.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn được yêu cầu cung cấp thông báo cho commit mới. Quá trình này chủ yếu viết lại lịch sử commit của bạn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Stash những thay đổi chưa được commit

Giả sử bạn đang làm việc trên một lỗi nhất định hoặc một tính năng, và bạn đột nhiên được yêu cầu để mô tả công việc của bạn. Công việc hiện tại của bạn chưa đủ để commit, và bạn không thể đưa ra ở giai đoạn này (mà không trở về những thay đổi). Trong tình huống như vậy, ```git stash``` đến để giải cứu bạn. Stash bản chất là thực hiện tất cả các thay đổi của bạn và lưu trữ chúng để sử dụng sau này. Để stash các thay đổi của bạn, bạn chỉ cần chạy lệnh sau:
```

git stash

```

Để kiểm tra danh sách các stash, bạn có thể chạy như sau:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn un-stash và phục hồi các thay đổi chưa commit, bạn gọi stash:

```

git stash apply

```

Trong ảnh chụp màn hình cuối cùng, bạn có thể thấy mỗi stash có một định danh, một số duy nhất (mặc dù chúng ta chỉ có một stash trong trường hợp này). Trong trường hợp bạn muốn áp dụng chỉ một stash được chọn , bạn thêm các định danh cụ thể vào lệnh ```apply``` :

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Kiểm tra những commit bị mất

Mặc dù ```reflog``` là một cách để kiểm tra các commit bị mất, nó không khả thi trong các repository lớn. Đây là khi lệnh ```fsck``` (kiểm tra hệ thống file) sẽ giúp bạn.

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy một commit bị mất. Bạn có thể kiểm tra các thay đổi trong commit bằng cách chạy ```git show [commit_hash]``` hoặc khôi phục nó bằng cách chạy ```git merge [commit_hash] ```.

```git fsck``` có lợi thế hơn```reflog```. Hãy nói rằng bạn đã xóa một remote branch và sau đó clone repository. Với ```fsck``` bạn có thể tìm kiếm và khôi phục các remote branch bị xóa.


##10. Cherry Pick

Cuối cùng tôi đã lưu lệnh Git thanh lịch nhất.
. Lệnh ```cherry-pick```là lệnh Git yêu thích nhất của tôi, vì ý nghĩa thực cũng như tiện ích của nó!

Trong thuật ngữ đơn giản, ```cherry-pick``` đang chọn một commit duy nhất từ một nhánh khác và kết hợp nó với một nhánh hiện tại. Nếu bạn đang làm việc theo kiểu song song trên hai hoặc nhiều nhánh, bạn có thể thấy một lỗi xảy ra ở tất cả các nhánh. Nếu bạn giải quyết nó trong một nhánh, bạn có thể cherry pick commit vào các nhánh khác, mà không ảnh hưởng đến các file hoặc commit khác.

Hãy xem xét một kịch bản mà chúng ta có thể áp dụng. Tôi có hai nhánh và tôi muốn ```cherry-pick``` commit ```b20fd14: Cleaned junk``` vào một cái khác.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi chuyển sang nhánh mà tôi muốn ```cherry-pick``` các commit và chạy lệnh sau:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù chúng tôi đã có một ```cherry-pick``` sạch, bạn nên biết rằng lệnh này có thể dẫn đến xung đột, do đó hãy cẩn thận khi sử dụng.

##Phần kết luận

Với điều này, chúng ta sẽ đến phần cuối của danh sách các mẹo mà tôi nghĩ rằng có thể giúp bạn nâng cao kỹ năng Git của mình. Git là thứ tốt nhất hiện có và nó có thể hoàn thành bất cứ điều gì bạn có thể tưởng tượng. Vì vậy, luôn luôn cố gắng để thử thách chính mình với Git. Rất có thể, bạn sẽ kết thúc việc học cái gì đó mới!

