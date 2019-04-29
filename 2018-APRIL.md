## Q1(a)(i)
```php
class Member extends Model {
  protected $fillable = ['account_no', 'name', 'phone'];
  public function transactions() {
    reeturn $this->hasMany(Transaction::class);
  }
}
```

## Q1(a)(ii)
```php
class Transaction extends Model {
  protected $fillable = ['code', 'points', 'member_id', 'created_at', 'updated_at'];
  public function member() {
    return $this->belongsTo(Member::class);
  }
}
```

## Q1(b)(i)
```php
$transactions = Transaction
  ::where([
    ['code'  , '=', 'EN'],
   ['points', '>', 1000]
  )
  ->orderBy('member_id')
  ->get();
```

## Q1(b)(ii)
```php
$points = Transaction::where('member_id', 2)->sum('points');
```

## Q1(b)(iii)
```php
$member->transactions->associate($transaction);
```

## Q1(b)(iv)
```php
$members = Member::withCount('transactions')->get();
foreach($members as $m) {
  echo $m->transactions_count;
}
```

## Q1(b)(v)
```php
$transactions = Transction::with(['member' => function($query) {
  $query->where('id', '=', 2);
}])->get();
foreach($transactions as $t) {
  echo $t->member->account_no;
}
```
