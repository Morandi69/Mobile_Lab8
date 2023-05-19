<p align = "center">МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ
РОССИЙСКОЙ ФЕДЕРАЦИИ
ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ
ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ
«САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ»</p>
<br><br><br><br><br><br>
<p align = "center">Институт естественных наук и техносферной безопасности<br>Кафедра информатики<br>Чагочкин Никита</p>
<br><br><br>
<p align = "center">Лабораторная работа №8.1<br>«Вывод списков и RecyclerView»<br>01.03.02 Прикладная математика и информатика</p>
<br><br><br><br><br><br><br><br><br><br><br><br>
<p align = "right">Научный руководитель<br>
Соболев Евгений Игоревич</p>
<br><br><br>
<p align = "center">г. Южно-Сахалинск<br>2023 г.</p>

***
# <p align = "center">Типы View в RecyclerView</p>
Для этого сложного упражнения вам нужно будет создать два типа строк в вашем RecyclerView: для обычных и для более серьезных преступлений. Чтобы это реализовать, вы будете работать с функцией в RecyclerView.Adapter. Присвойте новое свойство requiresPolice объекту Crime и используйте его, чтобы определить, какой тип View загружен в CrimeAdapter, путем реализации функции getItemViewType(Int) (developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html#getItemViewType). В функции onCreateViewHolder(ViewGroup, Int) вам также необходимо добавить логику, которая возвращает различные ViewHolder в зависимости от ViewType, возвращаемого функцией getItemViewType(Int). Используйте оригинальный макет для преступлений, которые не требуют вмешательства полиции, и новый макет с усовершенствованным интерфейсом, содержащий кнопку с надписью «Связаться с полицией» для серьезных преступлений.
***
# <p align = "center">Решение </p>
![](phone1.png)
## <p align = "center">Присвоил новое свойство requiresPolice объекту Crime </p>

        data class Crime(val id: UUID = UUID.randomUUID(),
                        var title: String = "",
                        var date: Date = Date(),
                        var isSolved: Boolean = false,
                        var requiresPolice:Boolean=false)

## <p align = "center">Реализовал функцию getItemViewType(Int) в функции onCreateViewHolder</p> 

        override fun getItemViewType(position: Int): Int {
            var crime=crimes[position]
            return if(crime.requiresPolice){
                1
            }else 0
        }

## <p align = "center">В функции onCreateViewHolder добавил логику, которая возвращает различные ViewHolder в зависимости от ViewType, возвращаемого функцией getItemViewType(Int).</p> 

        return if(viewType==1){
                var view = layoutInflater.inflate(R.layout.list_item_crime_police, parent, false)
                CrimeHolder(view)
            }else {
                var view = layoutInflater.inflate(R.layout.list_item_crime, parent, false)
                CrimeHolder(view)
            }

## <p align = "center">В файле CrimeListViewModel.kt добавил определение поля requiresPolice</p> 

        class CrimeListViewModel : ViewModel() {
            val crimes = mutableListOf<Crime>()
            init {
                for (i in 0 until 100) {
                    val crime = Crime()
                    crime.title = "Crime #$i"
                    crime.isSolved = i % 2 == 0
                    crime.requiresPolice=i%2!=0
                    crimes += crime
                }
            }
        }

## <p align = "center">Добавил новый макет с усовершенствованным интерфейсом, содержащий кнопку с надписью «Связаться с полицией» для серьезных преступлений.</p> 

        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:orientation="horizontal"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@drawable/dotted_border">
            <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:orientation="vertical"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:padding="8dp"
                android:layout_weight="2">
                <TextView
                    android:id="@+id/crime_title"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="Crime Title"/>
                <TextView
                    android:id="@+id/crime_date"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="Crime Date"/>
            </LinearLayout>

            <Button
                android:id="@+id/call_the_cops"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_marginTop="5dp"
                android:layout_marginBottom="5dp"
                android:layout_weight="2.8"
                android:text="call the cops"
                android:textSize="13dp"
                tools:ignore="TextSizeCheck,TouchTargetSizeCheck">

            </Button>
        </LinearLayout>

