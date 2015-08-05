**Rspec color and format**

```
rspec --color --format documentation spec/models/user_spec.rb 
```

```ruby
zombie = Zombie.new

### Next three lines equals to
zombie.iq.should == 0
zombie.eat_brains
zombie.iq.should == 3

#this one
expect{ zombie.eat_brains }.to change{ zombie.iq }.by(3)
```

```ruby
zombie = Zombie.new(name: 'Ash')
zombie.tweets.new(message: "Arrrgggggggghhhhh")
zombie.should have_at_least(1).tweets
```

```ruby
  zombie = Zombie.new
#before
  begin
    zombie.make_decision!
  rescue Zombie::NotSmartEnoughError => e
    e.should be_an_instance_of(Zombie::NotSmartEnoughError)
  end
  
#after
  expect{ zombie.make_decision! }.to raise_error(
    Zombie::NotSmartEnoughError
  )
```

```ruby
# After refactor
  context "when zombie has high iq" do
    subject { Zombie.new(iq: 3) }
    
    it { should be_genius }
    its(:brains_eaten_count) { should == 1 }
  end
  
# Before
  it "should be_genius with high iq" do
    zombie = Zombie.new(iq: 3)
    zombie.should be_genius
  end

  it 'should have a brains_eaten_count of 1 with high iq' do
    zombie = Zombie.new(iq: 3)
    zombie.brains_eaten_count.should == 1
  end
```