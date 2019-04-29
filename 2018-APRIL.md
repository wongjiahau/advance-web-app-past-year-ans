## Q1(a)(i)
```php
class Member extends Model {
  protected $fillable = ['account_no', 'name', 'phone'];
  public function transactions() {
    reeturn $this->hasMany(Transaction::class);
  }
}

class Transaction extends Model {
  protected $fillable = ['code', 'points', 'member_id', 'created_at', 'updated_at'];
  public function member() {
    return $this->belongsTo(Member::class);
  }
}
```
