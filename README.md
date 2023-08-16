В папке NewsPaper есть более легкочитаемый файл с командами shell - commands.txt
Миграции
(venv) PS C:\Users\user\PycharmProjects\pythonProject\D\D2\NewsPaper> py manage.py makemigrations
(venv) PS C:\Users\user\PycharmProjects\pythonProject\D\D2\NewsPaper> py manage.py migrate
Запуск shell
(venv) PS C:\Users\user\PycharmProjects\pythonProject\D\D2\NewsPaper> py manage.py shell

Импорт моделей
from news.models import *

Создаем пользователей
user1 = User.objects.create(username='Peter', first_name='Pavlov')
user2 = User.objects.create(username='Pavel', first_name='Petrov')

Создаем авторов
Author.objects.create(authorUser=user1)
Author.objects.create(authorUser=user2)

Создаем категории
Category.objects.create(category='Food')
Category.objects.create(category='Nature')
Category.objects.create(category='Kids')
Category.objects.create(category='Sport')

Создаем контент
Post.objects.create(author=Author.objects.get(authorUser=User.objects.get(username='Peter')), type='NW', title='КОРНАУХОВ НАЗВАЛ НЕОЖИДАННЫМ РЕШЕНИЕ ОБ ОТСТАВКЕ ТАШУЕВА', textPost='Олег Корнаухов заявил, что для него стало неожиданным решение об отставке Сергея Ташуева с поста главного тренера.')

Post.objects.create(author=Author.objects.get(authorUser=User.objects.get(username='Pavel')), type='AR', title='Ланч-бокс. Новый проект на телеканале', textPost='Телеканал Еда предлагает всем трудящимся разнообразить содержание своего ланч-бокса. Вкусная и сбалансированная еда – это то, на что остается так мало времени. Наш проект - отличный выход. Шеф Василий Емельяненко приготовит обед для всех, кто хорошо ест. Зрители смогут вдохновиться новым походом к комплексным обедам вне дома и готовить себе.')

Post.objects.create(author=Author.objects.get(authorUser=User.objects.get(username='Pavel')), type='AR', title='Природа – одна из самых удивительных вещей', textPost='Природа – одна из самых удивительных вещей. Как минимум, она существенно отличает Землю от других планет.')

Присваиваем категорию
Получаем посты
p1=Post.objects.get(pk=1) - новость, спорт, Петр
p2=Post.objects.get(pk=2) - статья, еда, Павел + спорт
p3=Post.objects.get(pk=3) - статья, природа, Павел

Получаем категории
c1=Category.objects.get(category='Sport')
c2=Category.objects.get(category='Food')
c3=Category.objects.get(category='Nature')
c4=Category.objects.get(category='Kids')

Связи
p1.category.add(c1)
p2.category.add(c2, c1)
p3.category.add(c3)

Создаем комментарии
Comment.objects.create(commentUser=User.objects.get(username='Pavel'), commentPost=Post.objects.get(pk=1), textComment='Безобразие!')
Comment.objects.create(commentUser=User.objects.get(username='Peter'), commentPost=Post.objects.get(pk=2), textComment='Буду смотреть')
Comment.objects.create(commentUser=User.objects.get(username='Pavel'), commentPost=Post.objects.get(pk=2), textComment='Будешь готовить!')
Comment.objects.create(commentUser=User.objects.get(username='Peter'), commentPost=Post.objects.get(pk=3), textComment='Природа - мать наша')

Лайки и дизлайки
Post.objects.get(pk=1).dislike()
Comment.objects.get(pk=1).like()

Post.objects.get(pk=2).like()
Comment.objects.get(pk=2).like()

Post.objects.get(pk=3).like()
Comment.objects.get(pk=3).like()

Обновляем рейтинги пользователей
Author.objects.get(authorUser=User.objects.get(username='Peter')).update_rating()
Author.objects.get(authorUser=User.objects.get(username='Pavel')).update_rating()

Выводим рейтинги
a=Author.objects.get(authorUser=User.objects.get(username='Peter'))
a.ratingAuthor
Результат: -1
b=Author.objects.get(authorUser=User.objects.get(username='Pavel'))
b.ratingAuthor
Результат: 10

Вывести имя и рейтинг лучшего пользователя
best_user=Author.objects.order_by('-ratingAuthor').first()
print('Самый высокий рейтинг у пользователя: ', best_user.authorUser.username, ', его рейтинг ', best_user.ratingAuthor)
Результат:
Самый высокий рейтинг у пользователя:  Pavel , его рейтинг  10

Лучшая статья
best_post=Post.objects.filter(type='AR').order_by('-ratingPost').first()
print('Лучшая статья добавлена ', best_post.timePost, 'автором ', best_post.author.authorUser.username, 'Рейтинг статьи: ', best_post.ratingPost, 'Заголовок: ', best_post.title, 'Превью: ', best_post.preview())
Результат:
Лучшая статья добавлена  2023-08-15 21:09:46.093120+00:00 автором  Pavel Рейтинг статьи:  1 Заголовок:  Ланч-бокс. Новый проект на телеканале Превью:  Телеканал Еда предлагает всем трудящимся разнообразить содержание своего ланч-бокса. Вкусная и сбалансированная еда – это то...

Выводим комментарии
Comment.objects.all().order_by().values('timeComment', 'commentUser__username', 'commentPost', 'ratingComment', 'textComment')[0]
Результат:
{'timeComment': datetime.datetime(2023, 8, 16, 10, 26, 55, 763918, tzinfo=datetime.timezone.utc), 'commentUser__username': 'Pavel', 'commentPost': 1, 'ratingComment': 1, 'textComment': 'Безобразие!'}
Comment.objects.filter().values('timeComment', 'commentUser__username', 'commentPost', 'ratingComment', 'textComment')[1]
Результат:
{'timeComment': datetime.datetime(2023, 8, 16, 10, 27, 27, 867059, tzinfo=datetime.timezone.utc), 'commentUser__username': 'Peter', 'commentPost': 2, 'ratingComment': 1, 'textComment': 'Буду смотреть'}
Comment.objects.filter().values('timeComment', 'commentUser__username', 'commentPost', 'ratingComment', 'textComment')[2]
Результат:
{'timeComment': datetime.datetime(2023, 8, 16, 10, 27, 42, 400557, tzinfo=datetime.timezone.utc), 'commentUser__username': 'Pavel', 'commentPost': 2, 'ratingComment': 1, 'textComment': 'Будешь готовить!'}
Comment.objects.filter().values('timeComment', 'commentUser__username', 'commentPost', 'ratingComment', 'textComment')[3]
Результат:
{'timeComment': datetime.datetime(2023, 8, 16, 10, 27, 56, 211290, tzinfo=datetime.timezone.utc), 'commentUser__username': 'Peter', 'commentPost': 3, 'ratingComment': 0, 'textComment': 'Природа - мать наша'}

