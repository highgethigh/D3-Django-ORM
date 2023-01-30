# D2-Django-ORM

## Запуск shell и импорт всех файлов
```
python3 manage.py shell
>>> from news.models import *
```
## 1. Создать двух пользователей (с помощью метода *User.objects.create()*).
```
>>> user_1 = User.objects.create(username='Alex', first_name='Alex', last_name='Pushkin')
>>> user_2 = User.objects.create(username='Elon', first_name='Elon', last_name='Musk')
```
## 2. Создать два объекта модели Author, связанные с пользователями.
```
>>> Author.objects.create(authors_link_user=user_1)
<Author: Author object (1)>
>>> Author.objects.create(authors_link_user=user_2)
<Author: Author object (2)>
```
## 3. Добавить 4 категории в модель Category.
```
>>> Category.objects.create(name_category='Poem')
<Category: Category object (1)>
>>> Category.objects.create(name_category='Space')
<Category: Category object (2)>
>>> Category.objects.create(name_category='Computer Science')
<Category: Category object (3)>
>>> Category.objects.create(name_category='AI')
<Category: Category object (4)>

```
## 4. Добавить 2 статьи и 1 новость.
```
>>> author = Author.objects.get(id=1)
>>> author2 = Author.objects.get(id=2)

>>> Post.objects.create(post_link_author = author, categoryType='A', title='Is it programming?',text='Computer programming is the process of performing a particular computation (or more generally, accomplishing a specific computing result), usually by designing and building an executable computer program.')
<Post: Post object (1)>

>>> Post.objects.create(post_link_author = author2, categoryType='A', title='Avatar: The Way of Water', text='Avatar: The Way of Water is a 2022 American epic science fiction film directed and produced by James Cameron, who co-wrote the screenplay with Rick Jaffa and Amanda Silver from a story the trio wrote with Josh Friedman and Shane Salerno. Distributed by 20th Century Studios, it is the sequel to Avatar (2009) and the second installment in the Avatar film series.')
<Post: Post object (2)>

>>> Post.objects.create(post_link_author = author, categoryType='N', title='Ilon Mask launches another rocket', text='He launched a super powerful rocket and it flew to a black hole.')
<Post: Post object (3)>
```
## 5. Присвоить им категории (как минимум в одной статье/новости должно быть не меньше 2 категорий).
```
>>> Post.objects.get(id=1).post_link_category.add(Category.objects.get(name_category='Poem'))
>>> Post.objects.get(id=1).post_link_category.add(Category.objects.get(name_category='Space'))
>>> Post.objects.get(id=2).post_link_category.add(Category.objects.get(name_category='Computer Science'))
>>> Post.objects.get(id=2).post_link_category.add(Category.objects.get(name_category='AI'))
>>> Post.objects.get(id=3).post_link_category.add(Category.objects.get(name_category='AI'))
>>> Post.objects.get(id=3).post_link_category.add(Category.objects.get(name_category='Space'))
```
## 6. Создать как минимум 4 комментария к разным объектам модели Post (в каждом объекте должен быть как минимум один комментарий).
```
>>> Comment.objects.create(comment_link_post=Post.objects.get(id=1), comment_link_user=Author.objects.get(id=1).authors_link_user, text_comment='it is interesting')
<Comment: Comment object (1)>
>>> Comment.objects.create(comment_link_post=Post.objects.get(id=2), comment_link_user=Author.objects.get(id=1).authors_link_user, text_comment='it is cool')
<Comment: Comment object (2)>
>>> Comment.objects.create(comment_link_post=Post.objects.get(id=3), comment_link_user=Author.objects.get(id=2).authors_link_user, text_comment='very beatiful')
<Comment: Comment object (3)>
>>> Comment.objects.create(comment_link_post=Post.objects.get(id=3), comment_link_user=Author.objects.get(id=1).authors_link_user, text_comment='Text is great')
<Comment: Comment object (4)>
```
## 7. Применяя функции like() и dislike() к статьям/новостям и комментариям, скорректировать рейтинги этих объектов.
```
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).like()
>>> Comment.objects.get(id=1).rating
6.0

>>> Comment.objects.get(id=2).dislike()
>>> Comment.objects.get(id=2).dislike()
>>> Comment.objects.get(id=2).like()
>>> Comment.objects.get(id=2).rating
-1.0

>>> Comment.objects.get(id=3).like()
>>> Comment.objects.get(id=3).rating
1.0

>>> Comment.objects.get(id=4).like()
>>> Comment.objects.get(id=4).like()
>>> Comment.objects.get(id=4).dislike()
>>> Comment.objects.get(id=4).dislike()
>>> Comment.objects.get(id=4).rating
0.0

>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).like()
>>> Post.objects.get(id=1).rating
8.0

>>> Post.objects.get(id=2).like()
>>> Post.objects.get(id=2).like()
>>> Post.objects.get(id=2).rating
2.0

>>> Post.objects.get(id=3).dislike()
>>> Post.objects.get(id=3).rating
-1.0
```
## 8. Обновить рейтинги пользователей.
```
>>> user1 = Author.objects.get(id=1)
>>> user1.update_rating()
>>> user1.author_rating
26.0

>>> user2 = Author.objects.get(id=2)
>>> user2.update_rating()
>>> user2.author_rating
7.0
```
## 9. Вывести *username* и рейтинг лучшего пользователя (применяя сортировку и возвращая поля первого объекта).
```
>>> best_user = Author.objects.all().order_by('-author_rating') [0]
>>> best_user
<Author: Author object (1)>

best_user.authors_link_user
<User: Alex>

>>> best_user.author_rating
26.0
```
## 10. Вывести дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи, основываясь на лайках/дислайках к этой статье.
```
>>> best_post = Post.objects.order_by('-rating') [0]
>>> best_post
<Post: Post object (1)>

>>> best_post.post_link_author
<Author: Author object (1)>


>>> best_post.post_data
datetime.datetime(2023, 1, 24, 18, 25, 35, 473782, tzinfo=datetime.timezone.utc)


>>> best_post.rating
8.0

>>> best_post.title
'Is it programming?'

>>> best_post.preview()
'Computer programming is the process of performing a particular computation (or more generally, accomplishing a specific com...'

----------------------------------------------------------------------------------------------------------------------------

>>> unbest_post = Post.objects.order_by('rating') [0]
>>> unbest_post
<Post: Post object (3)>

>>> unbest_post.post_link_author
<Author: Author object (1)>

>>> unbest_post.post_data
datetime.datetime(2023, 1, 24, 18, 27, 36, 30816, tzinfo=datetime.timezone.utc)

>>> unbest_post.rating
-1.0

>>> unbest_post.title
'Ilon Mask launches another rocket'

>>> unbest_post.preview()
'He launched a super powerful rocket and it flew to a black hole....'
```
## 11. Вывести все комментарии (дата, пользователь, рейтинг, текст) к этой статье.
```
>>> best_comment = Comment.objects.filter(comment_link_post=1)

>>> best_comment.values('create_data_comment', 'comment_link_user', 'rating', 'text_comment')
<QuerySet [{'create_data_comment': datetime.datetime(2023, 1, 24, 18, 31, 59, 460206, tzinfo=datetime.timezone.utc), 'comment_link_user': 1, 'rating': 6.0, 'text_comment': 'it is interesting'}]>
```

