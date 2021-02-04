***Unblocking Challenge 10***
This is my understanding of why my pair partner and I got blocked on challenge 10, and is shared with my pair partner and peer group. 

**Instruction:**

*Add a test to your DockingStation specification that a) gets a bike, and then b) expects the bike to be working*

*Make this test pass*

*Feature-test the feature again*

This is the test that we wrote: 

```ruby
it 'gets a bike that is working' do
     expects(bike.working?).to eq true
  end
```

The error in a feature test told us that we couldn't call `working?` on `bike` because `bike` was from the nil class. 

We knew from this that we needed to give the `bike` variable a value. 

We also tried to define bike within our methods in our classes. For example, we tried to create a new instance of `Bike` in the release bike method...

```ruby
class DockingStation

  def release_bike
    @bike = Bike.new
    @bike
  end
end
```

We spent time playing around in our classes. 

We were stuck on this error for ages: 

```ruby
Failures:

  1) DockingStation releases working bikes
     Failure/Error: expect(bike).to be_working # this is our expectation.

     NameError:
       undefined local variable or method `bike' for #<RSpec::ExampleGroups::DockingStation "releases working bikes" (./spec
/docking_station_spec.rb:6)>
     # ./spec/docking_station_spec.rb:8:in `block (2 levels) in <top (required)>'

Finished in 0.0157 seconds (files took 0.40407 seconds to load)
3 examples, 1 failure

Failed examples:

rspec ./spec/docking_station_spec.rb:6 # DockingStation releases working bikes
```

It had gotten to the end of the day and we were tired and had bleary eyes looking at the screen. We decided to look at the walkthrouugh... But it made no sense! So we decided to come back the next day. 

Here is the walkthrough. 

```ruby
describe DockingStation do
  it { is_expected.to respond_to :release_bike }

  it 'releases working bikes' do
    bike = subject.release_bike
    expect(bike).to be_working
  end
end
```

Our expectation looks different. That is because the walkthrough used a different matcher, for which there is an outline of the alternative syntax here: 

[https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/predicate-matchers](https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/predicate-matchers)

`subject` was confusing, but coach Eóin explained that this is 'out-of-the-box' syntax for a new instance of the class, in our case `Bike.new`. So, we cannot use another variable name here instead of subject, and this test could be written as: 

```ruby
require 'docking_station'

describe DockingStation do
  it { is_expected.to respond_to(:release_bike) }

  it 'releases working bikes' do
    bike = DockingStation.new.release_bike 
    expect(bike).to be_working 
  end
end
```

Now we get the error we were looking for: 

```ruby
Failures:

  1) DockingStation releases working bikes
     Failure/Error: expect(bike).to be_working
       expected nil to respond to `working?`
     # ./spec/docking_station_spec.rb:8:in `block (2 levels) in <top (required)>'

Finished in 0.12322 seconds (files took 0.41612 seconds to load)
3 examples, 1 failure

Failed examples:

rspec ./spec/docking_station_spec.rb:6 # DockingStation releases working bikes
```

and we can move to the next instructions:
*Make this test pass*

*Feature-test the feature again*
