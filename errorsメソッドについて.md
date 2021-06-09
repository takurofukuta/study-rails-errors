# errorsメソッドについて

## 目的
- ビューファイルにエラーメッセージを表示させる処理を書いたり、エラーメッセージの文字列を判定してバリデーションのテストをしたり、何かと使うerrorsメソッドに慣れる
- errorsメソッドに続くメソッド(add?, any?など)を知りたい

### そもそもバリデーションとは
- データベースにデータを保存する前に、保存内容を検証する機能
- バリデーションはモデルファイルに記述する
- バリデーションは、以下のメソッドがトリガーとなる
```ruby
save
save!
create
create!
update
update!

# バリデーションエラーが出ると、
# save, updateは false を返す
# createは、オブジェクトをそのまま返す
# !をつけると、保存されなかった場合にエラーメッセージが表示される。
```
- バリデーションエラーの有無は、`valid?`メソッドで判定できる
```ruby
user = User.new
user.valid?
# バリデーションエラーがある場合
# => true
# バリデーションエラーがない場合
# => false
```

### errorsメソッドとは
- エラーメッセージをハッシュ形式で取得する
- errors.messagesでエラーメッセージをハッシュで取得する
- errors.messages[:カラム名]でエラーメッセージ配列で取得する
- errors.full_messagesで全てのエラーメッセージを配列で取得する
- errors.full_messages_for(:カラム名)で特定のカラムに対するバリデーションエラーの全てメッセージを取得する
- errors.any?でエラーの有無を判定できる（any?メソッドは、モデルのデータが有る場合true、ない場合falseを返す）

```ruby
user = User.new
user.save

user.errors
=> #<ActiveModel::Errors:0x00007f9a889b9bd0 @base=#<User id: nil, user_name: nil, last_name: nil, first_name: nil, assignment_year: nil, created_at: nil, updated_at: nil, admin: false>, 
@errors=[
#<ActiveModel::Error attribute=password, type=blank, options={:if=>:password_required?}>, 
#<ActiveModel::Error attribute=user_name, type=blank, options={}>, 
#<ActiveModel::Error attribute=user_name, type=invalid, options={:value=>nil}>, 
#<ActiveModel::Error attribute=last_name, type=blank, options={}>, 
#<ActiveModel::Error attribute=first_name, type=blank, options={}>, 
#<ActiveModel::Error attribute=assignment_year, type=blank, options={}>, 
#<ActiveModel::Error attribute=encrypted_password, type=blank, options={}>
]>

user.errors.messages
=> {:password=>["を入力してください"], :user_name=>["を入力してください", "は不正な値です"], :last_name=>["を入力してください"], :first_name=>["を入力してください"], :assignment_year=>["を入力してください"], :encrypted_password=>["を入力してください"]}

user.errors.messages[:user_name]
=> ["を入力してください", "は不正な値です"]

user.errors.full_messages
=> ["パスワードを入力してください", "ユーザー名を入力してください", "ユーザー名は不正な値です", "苗字を入力してください", "名前を入力してください", "研究室配属年度を入力してください", "暗号化パスワードを入力してください"]

user.errors.full_messages_for(:user_name)
=> ["ユーザー名を入力してください", "ユーザー名は不正な値です"]

user.errors.any?
# => true
```

