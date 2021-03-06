
[中文版](./README-zh.md)
 | [日本語版](./README-ja.md)
 | [한국어](./README-ko.md)

[<img src="./images/elsewhen-logo.png" width="180" height="180">](http://elsewhen.co/)


# Project Guidelines &middot; [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
> While developing a new project is like rolling on a green field for you, maintaining it is a potential dark twisted nightmare for someone else.
Here's a list of guidelines we've found, written and gathered that (we think) works really well with most JavaScript projects here at [elsewhen](http://elsewhen.co).
If you want to share a best practice, or think one of these guidelines should be removed, [feel free to share it with us](http://makeapullrequest.com).
- [Git](#git)
    - [Some Git rules](#some-git-rules)
    - [Git workflow](#git-workflow)
    - [Writing good commit messages](#writing-good-commit-messages)
- [Documentation](#documentation)
- [Environments](#environments)
    - [Consistent dev environments](#consistent-dev-environments)
    - [Consistent dependencies](#consistent-dependencies)
- [Dependencies](#dependencies)
- [Testing](#testing)
- [Structure and Naming](#structure-and-naming)
- [Code style](#code-style)
    - [Some code style guidelines](#code-style-check)
    - [Enforcing code style standards](#enforcing-code-style-standards)
- [Logging](#logging)
- [API](#api)
    - [API design](#api-design)
    - [API security](#api-security)
    - [API documentation](#api-documentation)
- [Licensing](#licensing)

<a name="git"></a>
## 1. Git
![Git](/images/branching.png)
<a name="some-git-rules"></a>

### 1.1 Some Git rules
Đây là một bộ quy tắc để ghi nhớ:
* Thực hiện công việc trong một chi nhánh đặc trưng.
    
    _Tại sao:_
    >Bởi vì theo cách này tất cả công việc được thực hiện trên một nhánh đặc trưng thay vì nhánh chính. NÓ cho phép bạn gửi nhiều pull request mà không bị nhầm lẫn. Bạn có thể lặp với những code chưa hoàn thành có nguy cơ không ổn định mà không làm rối nhánh chính. [đọc thêm...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* Phát triển nhánh từ `develop`
    
    _Tại sao:_
    >Bằng cách này, bạn có thể chắc chắn rằng code trong nhánh master sẽ hầu như được build mà không có lỗi, và có thể được sử dụng trực tiếp cho bản phát hành (Điều này có thể quá sức đối với một vài dự án).

* Không bao giờ push vào nhánh `develop` hoặc `master`. Hãy tạo một pull request.
    
    _Tại sao:_
    > Nó thông báo cho thành viên của nhóm rằng chúng ta vừa hoàn thành một tính năng. Nó cho phép dẽ dàng bình duyệt lại mã và đưa ra diễn đàn thảo luận về tính năng được đề xuất.

* Cập nhật nhánh `develop` trên local và thực hiện một tương tác rebase trước khi push tính năng của bạn và tạo một Pull Request.

    _Tại sao:_
    > Rebase sẽ gộp vào nhánh được yêu cầu (`master` hoặc `develop`) và áp dụng các commit cái mà bạn đã tạo tại local lên đầu của lịch sử mà không tạo ra một merge commit (giả sử không có xung đột). Kết quả là một lịch sử tốt và rõ ràng. [ đọc thêm ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Xử lí xung đột tiềm năng trong khi rebase và trước khi tạo một Pull request.
* Xoá tính năng trên local và remote sau khi nhánh được merge.
    
    _Tại sao:_
    > Nó sẽ làm lẫn lộn danh sách nhánh của bạn với các nhánh chết. Nó đảm bảo bạn không bao giờ merge nhánh (`master` or `develop`) trở lại. Các nhánh đặc trưng chỉ nên tồn tại trong khi công việc vẫn đang trong tiến trình.

* Trước khi tạo một Pull Request, đẩm bảo rằng nhánh đặc trưng của bạn build thành công và qua tất cả các bài kiểm tra (bao gồm cả cách trình bày code).
    
    _Tại sao:_
    > Bạn chuẩn bị thêm code vào một nhánh ổn định.Nếu nhánh tính năng của bạn fail,Thì tỉ lệ cao nhánh đích của bạn cũng fail. Additionally, you need to apply code style check before making a Pull Request. It aids readability and reduces the chance of formatting fixes being mingled in with actual changes.
Ngoài ra, bạn cần áp dụng kiểm tra kiểu mã trước khi thực hiện một Pull Rrequest. Nó giúp dễ đọc và giảm tỉ lệ định dạng các bản sửa lỗi được trộn lẫn với những thay đổi thực tế.

* Sử dụng [cái này](./.gitignore) `.gitignore` file.
    
    _Why:_
    > Nó là một danh sách các file hệ thống không nên được gửi với code của bạn vào remote repo. Ngoài ra nó loại trừ các thư mục cài đặt và các file hầu như chỉ dùng cho các editor,cũng như các thư mục phổ biến nhất.

* Bảo vệ nhánh `develop` và `master` của bạn.
  
    _Tại sao:_

    > Nó bảo vệ các nhánh sẵn sàng từ các thay đổi không mong muốn và không thể đảo ngược. đọc thêm ... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) và [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

<a name="git-workflow"></a>
### 1.2 Quy trình làm việc Git
Vì hầu hết các lí do trên, chúng ta sử dụng [Quy trình làm việc với các nhánh tính năng](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) với [Tương tác Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) và một vài phần tử của [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (đặt tên và có một nhánh phát triển). Các bước chính như sau:

* Đối với dự án mới, khởi tạo một repo git trong thư mục của dự án. __Đối với các tính năng / thay đổi tiếp theo, bước này nên được bỏ qua__.
   ```sh
   cd <project directory>
   git init
   ```

* Kiểm tra một nhánh tính năng/sửa lỗi mới.
    ```sh
    git checkout -b <branchname>
    ```
* Thay đổi.
    ```sh
    git add
    git commit -a
    ```
    _Why:_
    > `git commit -a` sẽ bắt đầu một trình soạn thảo cho phép bạn tách đối tượng ra khỏi body. Đọc thêm về nó trong *section 1.3*.

* Đồng bộ với remote để lấy những thay đổi bạn đã bỏ lỡ.
    ```sh
    git checkout develop
    git pull
    ```
    
    _Tại sao:_
    > TĐiều này sẽ cho bạn cơ hội để đối phó với các xung đột trên máy tính của bạn trong khi rebasing (sau đó) thay vì tạo một Pull Request có chứa xung đột.
    
* Cập nhật nhánh tính năng của bạn với những thay đổi mới nhất từ phát triển bằng cách tương tác rebase.
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
    _Tại sao:_
    > Bạn có thể sử dụng --autosquash để làm lại tất cả các cmmit của bạn thành một commit duy nhất.Không ai muốn có quá nhiều commit cho  một tính năng trogn nhánh phát triển. [đọc thêm...](https://robots.thoughtbot.com/autosquashing-git-commits)
    
* Nếu bạn không có xung đột, bỏ qua bước này. Nếu ban có xung đột, [giải quyết chúng](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) và tiếp tục rebase.
    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```
* Push nhánh của bạn. Rebase sẽ thay đổi lịch sử, nên bạn phải sử dụng `-f` để buộc các thay đổi vào nhánh remote. Nếu ai đó đang làm việc trên nhánh của bạn, hãy sử dụng phương pháp ít gây tổn hại hơn `--force-with-lease`.
    ```sh
    git push -f
    ```
    
    _Why:_
    > WKhi bạn thực hiện một rebase, bạn đang thay đổi lịch sử trên nhánh chức năng của bạn.Kết quả là, Git sẽ từ chối push một cách bình thường `git push`. Thay vì vậy, bạn sẽ cần sử dụng -f hoặc --force cờ. [đọc thêm...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
    
* Tạo một Pull Request.
* Pull request sẽ được chấp nhận, được merge hoặc đóng bới người đánh giá.
* Gỡ nhánh tính năng trên local của bạn nếu bạn đã hoàn thành.

  ```sh
  git branch -d <branchname>
  ```
  để gỡ tất cả các nhánh không còn trên remote
  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>
### 1.3 Viết commit tốt

Having a good guideline for creating commits and sticking to it makes working with Git and collaborating with others a lot easier. Here are some rules of thumb ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):
Để có một hướng dẫn tốt cho việc tạo các commit và stichking để làm cho việc làm việc với Git và cộng tác với người khác dễ dàng hơn rất nhiều. Sau đây là một vài luật chính ([nguồn](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Tách đối tượng từ thân với một dòng mới ở giữa 2 phần.

    _Tại sao:_
    > Git đủ thông minh để phân biệt dòng đầu tiên commit dưới dạng tóm tắt của bạn. Thực tế, nếu bạn thử git shortlog, thay vì git log, bạn sẽ thấy một danh sách commit dài. bao gồm cả id của commit, và chỉ bản tổng hợp. 

 * Giới hạn là 50 với chủ đề vào 75 với nội dung.

    _why_
    > Các commit snên được mịn và tập trung nhất có thể, nó không phải là nơi được rườm rà. [đọc thêm...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

 * Tận dụng chủ đề.
 * Không kết thúc chủ đề với một khoảng thòi gian.
 * Sử dụng [câu mệnh lệnh](https://en.wikipedia.org/wiki/Imperative_mood) trong chủ đề.

    _Tại sao:_
    > Thay vì viết một tin nhắn nói rằng một commiter đã hoàn thành. Tốt hơn nên xem các thông báo này như là hướng dẫn cho những gì sẽ được thực hiện sau khi commit được áp dụng trên repo. [đọc thêm...](https://news.ycombinator.com/item?id=2079612)


 * Use the body to explain **what** and **why** as opposed to **how**.

 <a name="documentation"></a>
## 2. Tài liệu

![Tài liệu](/images/documentation.png)

* Sử dụng [mẫu này](./README.sample.md) cho `README.md`, Bạn có thể thêm các phần được để trống.
* Đối với các dự án có nhiều hơn một repo, cung cấp liên kết tới chúng trong các tệp `README.md` của chúng.
* Giữ cho `README.md` được cập nhật như là một dự án phát triển.
* Chú thích code của bạn. Cố gắng làm cho nó càng rõ ràng càng tốt những gì bạn đang có ý định với mỗi phần chính.
* Nếu có một cuộc thảo luận trên github hoặc stackoverflow về code hoặc cách tiếp cận mà bạn đang sử dụng, hãy bao gồm liên kết trong chú thích của bạn. 
* Đừng sử dụng chú thích như một cái cớ cho đoạn code xấu. Hãy giữ cho code của bạn luôn rõ ràng.
* Đừng sử dụng một đoạn code rõ ràng làm cái cớ để bạn không chú thích một cái gì.
* Giữ lại các chú thích có liên quan khi code của bạn phát triển.

<a name="environments"></a>
## 3. Environments

![Environments](/images/laptop.png)

* Define separate `development`, `test` and `production` environments if needed.

    _Why:_
    > Different data, tokens, APIs, ports etc... might be needed in different environments. You may want an isolated `development` mode that calls fake API which returns predictable data, making both automated and manual testing much easier. Or you may want to enable Google Analytics only on `production` and so on. [read more...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)


* Load your deployment specific configurations from environment variables and never add them to the codebase as constants, [look at this sample](./config.sample.js).

    _Why:_
    > You have tokens, passwords and other valuable information in there. Your config should be correctly separated from the app internals as if the codebase could be made public at any moment.

    _How:_
    >
    `.env` files to store your variables and add them to `.gitignore` to be excluded. Instead, commit a `.env.example`  which serves as a guide for developers. For production, you should still set your environment variables in the standard way.
    [read more](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

* It’s recommended to validate environment variables before your app starts.  [Look at this sample](./configWithTest.sample.js) using `joi` to validate provided values.
    
    _Why:_
    > It may save others from hours of troubleshooting.

<a name="consistent-dev-environments"></a>
### 3.1 Consistent dev environments:
* Set your node version in `engines` in `package.json`.
    
    _Why:_
    > It lets others know the version of node the project works on. [read more...](https://docs.npmjs.com/files/package.json#engines)

* Additionally, use `nvm` and create a  `.nvmrc`  in your project root. Don't forget to mention it in the documentation.

    _Why:_
    > Any one who uses `nvm` can simply use `nvm use` to switch to the suitable node version. [read more...](https://github.com/creationix/nvm)

* It's a good idea to setup a `preinstall` script that checks node and npm versions.

    _Why:_
    > Some dependencies may fail when installed by newer versions of npm.
    
* Use Docker image if you can.

    _Why:_
    > It can give you a consistent environment across the entire workflow. Without much need to fiddle with dependencies or configs. [read more...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)

* Use local modules instead of using globally installed modules.

    _Why:_
    > Lets you share your tooling with your colleague instead of expecting them to have it globally on their systems.


<a name="consistent-dependencies"></a>
### 3.2 Consistent dependencies:

* Make sure your team members get the exact same dependencies as you.

    _Why:_
    > Because you want the code to behave as expected and identical in any development machine [read more...](https://medium.com/@kentcdodds/why-semver-ranges-are-literally-the-worst-817cdcb09277)

    _how:_
    > Use `package-lock.json` on `npm@5` or higher

    _I don't have npm@5:_
    > Alternatively you can use `Yarn` and make sure to mention it in `README.md`. Your lock file and `package.json` should have the same versions after each dependency update. [read more...](https://yarnpkg.com/en/)

    _I don't like the name `Yarn`:_
    > Too bad. For older versions of `npm`, use `—save --save-exact` when installing a new dependency and create `npm-shrinkwrap.json` before publishing. [read more...](https://docs.npmjs.com/files/package-locks)

<a name="dependencies"></a>
## 4. Dependencies

![Github](/images/modules.png)

* Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
* See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)
    
    _Why:_
    > You may include an unused library in your code and increase the production bundle size. Find unused dependencies and get rid of them.

* Before using a dependency, check its download statistics to see if it is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)
    
    _Why:_
    > More usage mostly means more contributors, which usually means better maintenance, and all of these result in quickly discovered bugs and quickly developed fixes.

* Before using a dependency, check to see if it has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

    _Why:_
    > Having loads of contributors won't be as effective if maintainers don't merge fixes and patches quickly enough.

* If a less known dependency is needed, discuss it with the team before using it.
* Always make sure your app works with the latest version of its dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

    _Why:_
    > Dependency updates sometimes contain breaking changes. Always check their release notes when updates show up. Update your dependencies one by one, that makes troubleshooting easier if anything goes wrong. Use a cool tool such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

* Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).


<a name="testing"></a>
## 5. Kiểm thử
![Kiểm thử](/images/testing.png)
* Có một chế độ môi trường `test` nếu cần.

    _Tại sao:_
    > Trong khi đôi lúc kiểm thử cuối trong chế độ `production` dường như là đủ, thì lại có một số ngoại lệ:Một ví dụ là có thể bạn không muốn cho phép thông tin phân tích trên một chế độ `production` và là rối bảng điều khiển của ai đó với dữ liệu test. Một ví dụ khác là API của bạn có thể có các rate limit trong `production` và chặn các test call sau một lượng request nhất định.

* Đặt các file test của bạn cạnh các module đã được test sử dụng quy ước đặt tên `*.test.js` or `*.spec.js` ,  chẳng hạn `moduleName.spec.js`.

    _Tại sao:_
    > Bạn không muốn tìm kiếm thông qua một cấu trúc thư mục để tìm một bài kiểm tra đơn vị.[ đọc thêm...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)
    

* Đặt các file test bổ sung của bạn vào một thư mục test riêng biệt để tránh nhầm lẫn.

    _Tại sao:_
    > Một vài files thử nghiệm không đặc biệt liên quan đến bất kỳ file thực hiện cụ thể.Bạn phải đặt nó trong một thư mục có nhiều khả năng được tìm thấy bởi các nhà phát triển khác: thư mục `__test__`. Tên này: `__test__`  bây giờ cũng là chuẩn và được chọn bởi hầu hết các cuộc kiểm thử framework JavaScript.
* Viết code có thể kiểm chứng được, tránh tác dụng phụ , bỏ đi tác dụng phụ, viết các hàm thuần.

    _Tại sao:_
    > Bạn muốn thử nghiệm một business logic như các đơn vị riêng biệt. Bạn phải "giảm thiểu tác động của sự ngẫu nhiên và các quy trình không xác thực đối với độ tin cậy trong code của bạn". [đọc thêm...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)
    
    >Một hàm thuần là một hàm luôn trả về cùng output cho cùng một input. Ngược lại, một hàm không thuần là một hàm có thể có các tác dụng phụ hoặc phụ thuộc vào điều kiện từ bên ngoài để tạo ra giá trị. Điều đó làm cho nó không dự đoán được. [đọc thêm ...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

    _Tại sao:_
    >Đôi khi bạn cần một bộ kiểm tra kiểu tĩnh. Nó mang lại một mức độ tin cậy nhất định cho code của bạn.[đọc thêm...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)


* Chạy các test trên local trước khi thực hiện bất kỳ Pull request nào vào `develop`.

    _Tại sao:_
    > Bạn không muốn là người làm nên nhánh sẵn sàng sản xuất để bị lỗi. Chạy các test của bạnsau khi `rebase` và trước khi push nhánh chức năng lên remote repo.

* Tài liệu test của bạn bao gồm các hướng dẫn trong phần có liên quan của tập tin `README.md` của bạn.

    _Tại sao:_
    > Đây là một lưu ý tiện dụng bạn để lại cho các nhà phát triển khác hoặc chuyên gia DevOps hoặc QA hoặc bất cứ ai được may mắn đủ để làm việc trên code của bạn.

<a name="structure-and-naming"></a>
## 6. Cấu trúc và đặt tên
![Structure and Naming](/images/folder-tree.png)
* Tổ chức các tệp của bạn xung quanh các tính năng / trang / thành phần của sản phẩm chứ không phải vai trò. Ngoài ra, đặt các tệp kiểm tra của bạn bên cạnh việc triển khai.


    **Bad**

    ```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
    ```

    **Good**

    ```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
    ```

    _Tại sao:_
    > Instead of a long list of files, you will create small modules that encapsulate one responsibility including its test and so on. It gets much easier to navigate through and things can be found at a glance.
    Thay vì một danh sách dài các tệp tin, bạn sẽ tạo các mô-đun nhỏ nhận  một trách nhiệm bao gồm thử nghiệm của nó và vân vân. Nó dễ dàng điều hướng hơn và mọi thứ có thể được tìm thấy trong nháy mắt.

* Đặt các tệp thử nghiệm bổ sung của bạn vào một thư mục thử nghiệm riêng biệt để tránh nhầm lẫn.

    _Tại sao:_
    > Đó là tiết kiệm thời gian cho các chuyên gia phát triển khác hoặc chuyên gia DevOps trong nhóm của bạn.

* Sử dụng một thư mục `./config` và không tạo tệp cấu hình khác cho các môi trường khác nhau.

    _Tại sao:_
    >Khi bạn chia nhỏ tệp cấu hình cho các mục đích khác nhau (cơ sở dữ liệu, API và vân vân); đặt chúng vào một thư mục với một cái tên rất dễ nhận biết như ` config` làm cho nó có ý nghĩa. Chỉ cần nhớ không phải để làm cho các tập tin cấu hình khác nhau cho các môi trường khác nhau. Nó không quy mô rõ ràng, khi ứng dụng nhiều ứng dụng được tạo ra, tên môi trường mới là cần thiết.
Các giá trị được sử dụng trong tệp tin cấu hình nên được cung cấp bởi các biến môi trường. [đọc thêm...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)
    

* Đặt scripts cua bạn vào trong một thư mục `./scripts`. Nó bao gồm `bash` và `node` scripts.

    _Tại sao:_
    > Rất có thể bạn có thể kết thúc vớinhiều hơn một script, xây dựng sản xuất, phát triển xây dựng, cơ sở dữ liệu feeders, đồng bộ hóa cơ sở dữ liệu và hơn nữa.
    

* Đặt build output của bạn trong một thư mục `./build`. Thêm `build/` vào `.gitignore`.

    _Tại sao:_
    >Name it what you like, `dist` is also cool. But make sure that keep it consistent with your team. What gets in there is most likely generated  (bundled, compiled, transpiled) or moved there. What you can generate, your teammates should be able to generate too, so there is no point committing them into your remote repository. Unless you specifically want to. 
    > Đặt tên cho những gì bạn thích, dist cũng tốt. Nhưng hãy đảm bảo rằng giữ nó phù hợp với team của bạn. Những gì nhận được ở đó rất có thể được tạo ra (đóng gói, biên soạn, chuyển giao) hoặc chuyển đến đó. Những gì bạn có thể tạo ra,team của bạn cũng có thể tạo ra, do đó, không có point commit chúng vào remote repo của bạn. Trừ khi bạn đặc biệt muốn.

<a name="code-style"></a>
## 7. Phong cách code

![Code style](/images/code-style.png)

<a name="code-style-check"></a>
### 7.1 Một vài hướng dẫn phong cách code

* Sử dụng cú pháp JavaScript giai đoạn 2 và cao hơn (hiện đại) cho các dự án mới. Đối với dự án cũ ở phù hợp với cú pháp hiện tại trừ khi bạn có ý định hiện đại hóa dự án.

    _Tại sao:_
    > Tất cả tùy thuộc vào bạn. Chúng tôi sử dụng transpilers để sử dụng các lợi thế của cú pháp mới. giai đoạn 2 có nhiều khả năng trở thành một phần của spec với chỉ sửa đổi nhỏ.

* Bao gồm kiểm tra kiểu mã trong quy trình xây dựng của bạn.

    _Tại sao:_
    > Việc phá vỡ cấu trúc của bạn là một cách để thực thi kiểu mã vào mã của bạn. Nó ngăn cản bạn từ bỏ nó ít nghiêm túc. Làm điều đó cho cả client và server-side code. [đọc thêm...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

* Sử dụng [ESLint - Pluggable JavaScript linter](http://eslint.org/) để thực hiện phong cách code.

    _Tại sao:_
    > Chúng tôi chỉ đơn giản thích `eslint`, bạn không phải.Nó có nhiều quy tắc được hỗ trợ, khả năng cấu hình các quy tắc và khả năng thêm các quy tắc tùy chỉnh.

* Chúng ta sử dụng [Hướng dẫn về phong cách JavaScript Airbnb](https://github.com/airbnb/javascript) cho JavaScript, [đọc thêm...](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Sử dụng hướng dẫn style javascript theo yêu cầu của dự án hoặc nhóm của bạn.

* Chúng ta sử dụng [Quy tắc kiểm tra kiểu kiểu luồng cho ESLint](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).

    _Tại sao:_
    > luồng giới thiệu vài cú pháp cũng cần phải làm theo một số kiểu mã và được kiểm tra.

* Sử dụng `.eslintignore` để loại trừ các tệp tin hoặc thư mục khỏi code style checks.

    _Tại sao:_
    > Bạn không cần phải gây rối code của bạn với các comment `eslint-disable`bất cứ khi nào bạn cần loại trừ một vài tập tin từ style checking.

* Xóa bất kỳ nhận xét nào bị tắt của bạn khỏi `eslint` trước khi thực hiện Pull request.

    _Tại sao:_
    > Thật bình thường để kiểm tra tính năng không hoạt động trong khi làm việc trên một khối mã để tập trung nhiều hơn vào logic. Chỉ cần nhớ xóa những `eslint-disable` này và thực hiện theo các quy tắc.

* Tùy thuộc vào kích thước của nhiệm vụ sử dụng  `// TODO:` comment hoặc mở một ticket.

   _Tại sao:_ 

    > Vì sau đó bạn có thể nhắc nhở bản thân hoặc người khác về các công việc nhỏ ( như là sắp xếp lại các hàm hoặc cập nhật các comment). Với các công việc lớn hơn sử dụng `//TODO(#3456)` được thực hiện bởi các luật và con số là 1 thẻ mở.

  

* Luôn luôn comment liên quan những thay đổi của code. Loại bỏ các đoạn code được comment.

     

    _Tại sao:_ 

    > Code của bạn nênên càng dễ đọc càng tốt, bạn nên thoát ra khỏi những thứ gây xao nhãng. Nếu bạn sắp xếp lại các hàm, đừng chỉ comment những cái cũ, hãy xóa chúng đi. 

  

* Tránh những comment, log hay cách đặt tên vui vẻ hay không liên quan.

  

    _Tại sao:_ 

    > Khi mà cái quy trình kiến trúc của bạn có thể(nên) tránh xa khỏi chúng, đôi khi mã nguồn của bạn có thể được bàn giao cho công ty/ khách hàng khác mà họ có thể không cười nhạo chúng.

  

* Đặt tên  dễ tìm kiếm với nhưng ý nghĩa phân biệt tránh tên ngắn. Với các hàm sử dụng các tên dài, mang nghĩa mô tả. Tên của các hàm nên là 1 động từ hoặc cụm động từ, và nó cần liên quan đến chủ đích của hàm đó.

  

      _Tại sao:_ 

    > Nó làm cho việc đọc mã nguồn tự nhiên hơn.

  

* Tổ chức các hàm của bạn trong 1 file dựa theo luật step-down. Những hàm ở mức cao hơn nên ở trên cùng và mức thấp thì ở bên dưới.

  

    _Tại sao:_ 

    > Nó làm cho việc đọc mã nguồn tự nhiên hơn.

  

<a name="enforcing-code-style-standards"></a> 

### 7.2 Áp dụng các chuẩn phong cách lập trình 

  
* Sử dụng 1 file [.editorconfig](http://editorconfig.org/) để giúp các lập trình viên định nghĩa và bảo trì phong cách lập trình thích hợp giữa các trình soạn thảo và các IDE khác nhau trên dự án

  

    _Tại sao:_ 
    > Dự án EditorConfig bao gồm các định dạng file để định nghĩa các phong cách lập trình và 1 tập các plugin của các trình soạn thảo văn bản cho phép các trình soạn thảo đọc các định dạng file và tuân theo để định nghĩa các phong cách, Các file EditorConfig cũng rất dễ đọc và nó cũng tương tác hiệu quả với các hệ thống quản lý phiên bản.

  

* Để các trình soạn thảo thông báo cho bạn về các lỗi phong  cách lập trình. Sử dụng [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) và [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) với các cấu hình ESLint hiện tại của bạn. [đọc thêm...](https://github.com/prettier/eslint-config-prettier#installation) 

  

* Xem xét sử dụng Git hooks.

  

    _Tại sao:_ 


    > Git hooks sẽ tăng mạnh năng suất của các lập trình viên. Chúng ta có thể tạo các thay đổi, commit và push lên môi trường staging hoặc production mà không sợ phá vỡ kiến trúc. [đọc thêm...](http://githooks.com/) 

  

* Sử dụng Prettier với precommit hook

  

    _Tại sao:_ 

    > Vì bản thân `prettier` nó rất mạnh, nên việc đơn giản chạy nó như nhưng công việc npm độc lập mỗi lần để chỉnh sửa code sẽ không năng suất lắm. Đây cũng là lúc mà `lint-staged` ( và `husky`) trở nên quan trọng. Đọc thêm về cấu hình `lint-staged` [ở đây](https://github.com/okonet/lint-staged#configuration)
 và cấu hình `husky` [ở đây](https://github.com/typicode/husky).
  

  

<a name="logging"></a> 

## 8. Ghi log 

  

![Ghi log](/images/logging.png) 

  

* Tránh các in ra log trong môi trường sản xuất bên phía client

  

    _Tại sao:_ 

    > Mặc dù quá trình xây dựng của bạn có thể (nên) thoát khỏi chúng, hãy chắc rằng bộ kiểm tra phong cách lập trình của bạn cảnh báo bạn về việc in log ra bên ngoài.

  

* Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or 
[node-bunyan](https://github.com/trentm/node-bunyan)). 
* Tạo ra các log production dễ đọc. Lý tưởng nhất là sử dụng thửz viện ghi log trong chế độ production ( ví dụ như [winston](https://github.com/winstonjs/winston) hoặc 
[node-bunyan](https://github.com/trentm/node-bunyan)).


  

    _Why:_ 

    > It makes your troubleshooting less unpleasant with colorization, timestamps, log to a file in addition to the console or even logging to a file that rotates daily. [read more...](https://blog.risingstack.com/node-js-logging-tutorial/) 

  

  

<a name="api"></a>   
## 8. Logging

![Logging](/images/logging.png)

* Avoid client-side console logs in production

    _Why:_
    > Even though your build process can (should) get rid of them, make sure that your code style checker warns you about leftover console logs.

* Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or
[node-bunyan](https://github.com/trentm/node-bunyan)).

    _Why:_
    > It makes your troubleshooting less unpleasant with colorization, timestamps, log to a file in addition to the console or even logging to a file that rotates daily. [read more...](https://blog.risingstack.com/node-js-logging-tutorial/)


<a name="api"></a>
## 9. API
<a name="api-design"></a>

![API](/images/api.png)

### 9.1 API design

_Why:_
> Because we try to enforce development of sanely constructed RESTful interfaces, which team members and clients can consume simply and consistently.  

_Why:_
> Lack of consistency and simplicity can massively increase integration and maintenance costs. Which is why `API design` is included in this document.


* We mostly follow resource-oriented design. It has three main factors: resources, collection, and URLs.
    * A resource has data, gets nested, and there are methods that operate against it.
    * A group of resources is called a collection.
    * URL identifies the online location of resource or collection.
    
    _Why:_
    > This is a very well-known design to developers (your main API consumers). Apart from readability and ease of use, it allows us to write generic libraries and connectors without even knowing what the API is about.

* use kebab-case for URLs.
* use camelCase for parameters in the query string or resource fields.
* use plural kebab-case for resource names in URLs.

* Always use a plural nouns for naming a url pointing to a collection: `/users`.

    _Why:_
    > Basically, it reads better and keeps URLs consistent. [read more...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)

* In the source code convert plurals to variables and properties with a List suffix.

    _Why_:
    > Plural is nice in the URL but in the source code, it’s just too subtle and error-prone.

* Always use a singular concept that starts with a collection and ends to an identifier:

    ```
    /students/245743
    /airports/kjfk
    ```
* Avoid URLs like this: 
    ```
    GET /blogs/:blogId/posts/:postId/summary
    ```

    _Why:_
    > This is not pointing to a resource but to a property instead. You can pass the property as a parameter to trim your response.

* Keep verbs out of your resource URLs.

    _Why:_
    > Because if you use a verb for each resource operation you soon will have a huge list of URLs and no consistent pattern which makes it difficult for developers to learn. Plus we use verbs for something else.

* Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result. These **are not** CRUD (create, retrieve, update, and delete) operations:

    ```
    /translate?text=Hallo
    ```

    _Why:_
    > Because for CRUD we use HTTP methods on `resource` or `collection` URLs. The verbs we were talking about are actually `Controllers`. You usually don't develop many of these. [read more...](https://byrondover.github.io/post/restful-api-guidelines/#controller)

* The request body or response type is JSON then please follow `camelCase` for `JSON` property names to maintain the consistency.
    
    _Why:_
    > This is a JavaScript project guideline, where the programming language for generating and parsing JSON is assumed to be JavaScript.

* Even though a resource is a singular concept that is similar to an object instance or database record, you should not use your `table_name` for a resource name and `column_name` resource property.

    _Why:_
    > Because your intention is to expose Resources, not your database schema details.

* Again, only use nouns in your URL when naming your resources and don’t try to explain their functionality.

    _Why:_
    > Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` .  Also avoid sending resource operations as a parameter.

* Explain the CRUD functionalities using HTTP methods:

    _How:_
    > `GET`: To retrieve a representation of a resource.
    
    > `POST`: To create new resources and sub-resources.
   
    > `PUT`: To update existing resources.
    
    > `PATCH`: To update existing resources. It only updates the fields that were supplied, leaving the others alone.
    
    > `DELETE`:	To delete existing resources.


* For nested resources, use the relation between them in the URL. For instance, using `id` to relate an employee to a company.

    _Why:_
    > This is a natural way to make resources explorable.

    _How:_

    > `GET      /schools/2/students	` , should get the list of all students from school 2.

    > `GET      /schools/2/students/31`	, should get the details of student 31, which belongs to school 2.

    > `DELETE   /schools/2/students/31`	, should delete student 31, which belongs to school 2.

    > `PUT      /schools/2/students/31`	, should update info of student 31, Use PUT on resource-URL only, not collection.

    > `POST     /schools` , should create a new school and return the details of the new school created. Use POST on collection-URLs.

* Use a simple ordinal number for a version with a `v` prefix (v1, v2). Move it all the way to the left in the URL so that it has the highest scope:
    ```
    http://api.domain.com/v1/schools/3/students	
    ```

    _Why:_
    > When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening. [read more...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)



* Response messages must be self-descriptive. A good error message response might look something like this:
    ```json
    {
        "code": 1234,
        "message" : "Something bad happened",
        "description" : "More details"
    }
    ```
    or for validation errors:
    ```json
    {
        "code" : 2314,
        "message" : "Validation Failed",
        "errors" : [
            {
                "code" : 1233,
                "field" : "email",
                "message" : "Invalid email"
            },
            {
                "code" : 1234,
                "field" : "password",
                "message" : "No password provided"
            }
          ]
    }
    ```

    _Why:_
    > developers depend on well-designed errors at the critical times when they are troubleshooting and resolving issues after the applications they've built using your APIs are in the hands of their users.


    _Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only the password was incorrect._

* Use these status codes to send with your response to describe whether **everything worked**,
The **client app did something wrong** or The **API did something wrong**.
    
    _Which ones:_
    > `200 OK` response represents success for `GET`, `PUT` or `POST` requests.

    > `201 Created` for when a new instance is created. Creating a new instance, using `POST` method returns `201` status code.

    > `204 No Content` response represents success but there is no content to be sent in the response. Use it when `DELETE` operation succeeds.

    > `304 Not Modified` response is to minimize information transfer when the recipient already has cached representations.

    > `400 Bad Request` for when the request was not processed, as the server could not understand what the client is asking for.

    > `401 Unauthorized` for when the request lacks valid credentials and it should re-request with the required credentials.

    > `403 Forbidden` means the server understood the request but refuses to authorize it.

    > `404 Not Found` indicates that the requested resource was not found. 

    > `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.

    _Why:_
    > Most API providers use a small subset HTTP status codes. For example, the Google GData API uses only 10 status codes, Netflix uses 9, and Digg, only 8. Of course, these responses contain a body with additional information.There are over 70 HTTP status codes. However, most developers don't have all 70 memorized. So if you choose status codes that are not very common you will force application developers away from building their apps and over to wikipedia to figure out what you're trying to tell them. [read more...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)


* Provide total numbers of resources in your response.
* Accept `limit` and `offset` parameters.

* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource. Use a fields query parameter that takes a comma separated list of fields to include:
    ```
    GET /student?fields=id,name,age,class
    ```
* Pagination, filtering, and sorting don’t need to be supported from start for all resources. Document those resources that offer filtering and sorting.

<a name="api-security"></a>
### 9.2 API security
These are some basic security best practices:

* Don't use basic authentication unless over a secure connection (HTTPS). Authentication tokens must not be transmitted in the URL: `GET /users/123?token=asdf....`

    _Why:_
    > Because Token, or user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. [read more...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

* Tokens must be transmitted using the Authorization header on every request: `Authorization: Bearer xxxxxx, Extra yyyyy`.

* Authorization Code should be short-lived.

* Reject any non-TLS requests by not responding to any HTTP request to avoid any insecure data exchange. Respond to HTTP requests by `403 Forbidden`.

* Consider using Rate Limiting.

    _Why:_
    > To protect your APIs from bot threats that call your API thousands of times per hour. You should consider implementing rate limit early on.

* Setting HTTP headers appropriately can help to lock down and secure your web application. [read more...](https://github.com/helmetjs/helmet)

* Your API should convert the received data to their canonical form or reject them. Return 400 Bad Request with details about any errors from bad or missing data.

* All the data exchanged with the REST API must be validated by the API.

* Serialize your JSON.

    _Why:_
    > A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser... or, if you're using node.js, on the server. It's vital that you use a proper JSON serializer to encode user-supplied data properly to prevent the execution of user-supplied input on the browser.

* Validate the content-type and mostly use `application/*json` (Content-Type header).
    
    _Why:_
    > For instance, accepting the `application/x-www-form-urlencoded` mime type allows the attacker to create a form and trigger a simple POST request. The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `4XX` response.

* Check the API Security Checklist Project. [read more...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>
### 9.3 API documentation
* Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
* Describe API authentication methods with a code sample.
* Explaining The URL Structure (path only, no root URL) including The request type (Method).

For each endpoint explain:
* URL Params If URL Params exist, specify them in accordance with name mentioned in URL section:

    ```
    Required: id=[integer]
    Optional: photo_id=[alphanumeric]
    ```

* If the request type is POST, provide working examples. URL Params rules apply here too. Separate the section into Optional and Required.

* Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect:

    ```
    Code: 200
    Content: { id : 12 }
    ```

* Error Response, Most endpoints have many ways to fail. From unauthorized access to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }   
    ```


* Use API design tools, There are lots of open source tools for good documentation such as [API Blueprint](https://apiblueprint.org/) and [Swagger](https://swagger.io/).

<a name="licensing"></a>
## 10. Licensing
![Licensing](/images/licensing.png)

Make sure you use resources that you have the rights to use. If you use libraries, remember to look for MIT, Apache or BSD but if you modify them, then take a look at the license details. Copyrighted images and videos may cause legal problems.


---
Sources:
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials),
[Apigee](https://apigee.com/about/blog),
[Wishtack](https://blog.wishtack.com)

Icons by [icons8](https://icons8.com/)

