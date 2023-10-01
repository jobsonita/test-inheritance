# Code:

```ts
class Entity<Props> {
  private _id: number;
  protected _props: Props;

  get id() {
    return this._id;
  }

  get props(): Props {
    return this._props;
  }

  protected constructor(props: Props, id?: number) {
    this._props = props;
    this._id = id ?? -Math.random() * 1000000;
  }

  public equals(entity: Entity<unknown>) {
    if (entity === this) return true;

    if (entity.id === this._id) return true;

    return false;
  }
}

interface UserProps {
  email: string;
  password: string;
}

class User<Props extends UserProps> extends Entity<Props> {
  constructor(props: Props, id?: number) {
    super(props, id);
  }

  static create<T extends User<U>, U extends UserProps>(
    cls: new (props: U, id?: number) => T,
    props: U,
    id?: number
  ) {
    const user = new cls(props, id);
    return user;
  }

  static createUser(props: UserProps, id?: number) {
    return User.create(User, props, id);
  }
}

interface PlayerProps extends UserProps {
  firstName: string;
  lastName: string;
}

class Player extends User<PlayerProps> {
  constructor(props: PlayerProps, id?: number) {
    super(props, id);
  }

  static createPlayer(props: PlayerProps, id?: number) {
    return User.create(Player, props, id);
  }
}


const playerData: PlayerProps = {
  email: "email",
  password: "password",
  firstName: "firstName",
  lastName: "lastName",
};

const userData: UserProps = {
  email: "email",
  password: "password",
};


/* Remove the comments below and see which fields are available on each props */

const player1 = Player.createPlayer(playerData, 1);
// player1.props.

console.log('player1: ', player1.props);
console.log('is User:', player1 instanceof Player);
console.log('is Player:', player1 instanceof User);

const player2 = User.create(Player, playerData, 2);
// player2.props.

console.log('\nplayer2: ', player2.props);
console.log('is User:', player2 instanceof User);
console.log('is Player:', player2 instanceof Player);

const user1 = User.create(User, userData, 3);
// user1.props.

console.log('\nuser1: ', user1.props);
console.log('is User:', user1 instanceof User);
console.log('is Player:', user1 instanceof Player);

const user2 = User.create(User, playerData, 4);
// user2.props.

console.log('\nuser2: ', user2.props);
console.log('is User:', user2 instanceof User);
console.log('is Player:', user2 instanceof Player);

const user3 = User.create<User<UserProps>, UserProps>(User, playerData, 4);
// user3.props.

console.log('\nuser3: ', user3.props);
console.log('is User:', user3 instanceof User);
console.log('is Player:', user3 instanceof Player);

const user4 = User.createUser(userData, 5);
// user4.props.

console.log('\nuser4: ', user4.props);
console.log('is User:', user4 instanceof User);
console.log('is Player:', user4 instanceof Player);

// In the line below, TypeScript will throw an error because the createPlayer
// method expects a PlayerProps object

// Player.createPlayer(userData, 6);

const user5 = User.createUser(playerData, 7);
// user5.props.

console.log('\nuser5: ', user5.props);
console.log('is User:', user5 instanceof User);
console.log('is Player:', user5 instanceof Player);

const user6 = Player.create(User, userData, 8);
// user6.props.

console.log('\nuser6: ', user6.props);
console.log('is User:', user6 instanceof User);
console.log('is Player:', user6 instanceof Player);
```

# Output:

```bash
player1:  {
  email: 'email',
  password: 'password',
  firstName: 'firstName',
  lastName: 'lastName'
}
is User: true
is Player: true

player2:  {
  email: 'email',
  password: 'password',
  firstName: 'firstName',
  lastName: 'lastName'
}
is User: true
is Player: true

user1:  { email: 'email', password: 'password' }
is User: true
is Player: false

user2:  {
  email: 'email',
  password: 'password',
  firstName: 'firstName',
  lastName: 'lastName'
}
is User: true
is Player: false

user3:  {
  email: 'email',
  password: 'password',
  firstName: 'firstName',
  lastName: 'lastName'
}
is User: true
is Player: false

user4:  { email: 'email', password: 'password' }
is User: true
is Player: false

user5:  {
  email: 'email',
  password: 'password',
  firstName: 'firstName',
  lastName: 'lastName'
}
is User: true
is Player: false

user6:  { email: 'email', password: 'password' }
is User: true
is Player: false
```
