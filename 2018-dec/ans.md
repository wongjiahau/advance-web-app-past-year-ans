## Q1(a)(i)
```php
class Student extends Model {
  protected $fillable = ['student_no', 'name', 'email', 'phone', 'gender'];
  public function courses() {
    return $this->belongsToMany(Course::class);
  }
}
```
## Q1(a)(ii)
```php
class Course extends Model {
  protected $fillable = ['code', 'title', 'credit_hours'];
  public function students() {
    return $this->belongsToMany(Student::class);
  }
}
```
