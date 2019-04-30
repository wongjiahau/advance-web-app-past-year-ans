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

## Q2(a)(i)
```php
public function create() {
  return view('customer.create');
}
```

## Q2(a)(ii)
```php
public function store(Request $r) {
  $r->validate([
    'customer_no'=> 'required|string|unique:customers,customer_no|max:10',
    'name'       => 'required|string|max:200',
    'email'      => 'required|email|max:200',
    'phone'      => 'required|digits:20',
    'address'    => 'required|string|max:500',
    'postcode'   => 'required|digits:5',
    'city'       => 'required|string|max:100',
    'state'      => 'required|string|min:2|max:2'
  ]);
  $c = Customer::create($r->all());
  return response()->json([
    'id' => $c->id,
    'created_at' => $c->created_at
  ], 201);
}
```

## Q2(a)(iii)
```php
public function getall() {
  return Customer::all();
}
```

## Q2(a)(iv)
```php
public function getone($id) {
  $c = Customer::find($id);
  if($c) {
    return $c;
  } else {
    return response()->json(['error' => 'Not found'], 404);
  }
}
```

## Q2(b)
```php
Route::get ('/customers/index' , 'CustomerController@create');
Route::post('/customers'       , 'CustomerController@store' );
Route::get ('/customers'       , 'CustomerController@getall');
Route::get ('/customers/{id}'  , 'CustomerController@getone');
```

## Q2(c)
Skipped.

## Q3(a)
```php
public function isAdmin() {
  return $this->access_level === 2;
}

public function isSuperAdmin() {
  return $this->access_level === 1;
}
```

## Q3(b)
```php
class AlbumPolicy {
  public function before($user, $ability) {
    if($user->isAdmin() || $user->isSuperAdmin()) {
      return true;
    }
  }
  
  public function create(User $u) {
    // should be empty
  }
  
  public function edit(User $u, Album $a) {
    return $u->id === $a->user_id;
  }
  
  public function delete(User $u, Album $a) {
    return $u->id === $a->user_id;
  }
  
  public function upload(User $u, Album $a) {
    return $u->id === $a->user_id;
  }
}
```

## Q3(c)(i)
```php
if($user->can('create', Album::class)) { 
  // do something
}
```

## Q3(c)(ii)
```php
Route::put('album/{id}', 'AlbumController@edit', function(Album $a){})->middleware('can:edit,album');
```
## Q3(c)(iii)
```php
class AlbumController extends Controller {
  public function upload(Request $r, Album $a) {
    $this->authorize('upload', $a);
  }
}
```

## Q4(a)
```js
class CustomerTable extends Component {
  constructor() {
    this.state = JSON.parse(rawData); // assume rawData is defined
  }
  
  render() {
    return (
      <div>
        <span style={textAlign:'center'}><i>List of Customers of My Cat Shop</i></span><br>
        <table>
          <thead>
            <tr>
              <th>No.</th>
              <th>Customer No.</th>
              <th>Customer Name</th>
              <th>Zone</th>
            </tr>
          </thead>
          <tbody>
            {
              this.state.map((c, index) => 
                (
                  <tr>
                    <td>{index}</td>
                    <td>{c.customer_no}</td>
                    <td>{c.customer_name}</td>
                    <td>{c.zone}</td>
                  </tr>
                )
              )
            }
          </tbody>
        </table>
      </div>
    );
  }
}
```

## Q4(b)
```js
class WeatherView extends Component {
  constructor() {
    this.state = {
      city:  '',
      day:   null,
      night: null,
      error: null
    };
  }
  
  componentDidMount() {
    fetch("/weather/get-data?location=07GT", {
      method: "GET",
      headers: {
        'Accept': 'application/json'
      }
    })
    .then(r => {
      if(r.status === 200) {
        return r.json();
      } else {
        throw new Error(r.status + r.statusText);
      }
    })
    .then(data => {
      this.setState(data);
    })
    .catch(error => {
      this.setState({error: error.message});
    });
  }
  
  render() {
    return (
      <div>
        <p id="error-message">{this.state.error}</p>
        <span style={textAlign:'center'}><i>Weather for {this.state.city} </i></span><br>
        <table>
          <tbody>
            <tr>
              <td>day</td>
              <td>
                <ul>
                  <li>{this.state.day.status}</li>
                  <li>{this.state.day.temperature + String.fromCharCode(176)}</li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>night</td>
              <td>
                <ul>
                  <li>{this.state.night.status}</li>
                  <li>{this.state.night.temperature + String.fromCharCode(176)}</li>
                </ul>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    );
  }
}
```

## Q5(a)
```js
class MyView extends Component {
  constructor() {
    this.state = bookData; // assume bookData is predefined
  }
  
  render() {
    return (
      <ol type="1">
        {this.state.map(book =>(
          <li>{book.authors.join(",")}<i>{book.title}</i></li>
        ))}
      </ol>
    );
  }
}
```

## Q5(B)
The answer for this question is incomplete.
```js
class MyForm extends Component {
  constructor() {
    this.state = {
      username: '',
      email_domain: '',
      password: '',
      password_confirm: '',
      firstname: '',
      lastname: '',
      country: ''
    };
  }
  
  handleChange = (fieldName) => (event) => {
    this.setState({[fieldName]: event.target.value});
  }

  handleCheckAvail = () => {
    fetch("/user/checkavail?emaildomain=" + this.state.email_domain , {
      method: "GET",
      headers: {
        'Accept': 'application/json'
      }
    })
    .then(r => {
      if(r.status === 200) return r.json();
      else throw new Error(r.status + r.statusText);
    })
    .then(data => ?????????????????
  }
  
  render() {
     return (
        <form onSubmit={(e) => e.preventDefault()}>
          <label>Username:         <input type="text"     onChange={this.handleChange("username")}>        </label>
          <label>Email domain:     <input type="text"     onChange={this.handleChange("email_domain")}>    </label>
          <label>Password:         <input type="password" onChange={this.handleChange("password")}>        </label>
          <label>Confirm password: <input type="password" onChange={this.handleChange("password_confirm")}></label>
          <label>First name:       <input type="text"     onChange={this.handleChange("firstname")}>       </label>
          <label>Last name:        <input type="text"     onChange={this.handleChange("lastname")}>        </label>
          <label>Country:          <input type="text"     onChange={this.handleChange("country")}>         </label>
          <button onClick={this.handleCheckAvail}>Check availability</button>
        </form>
     );
  }
}
```
