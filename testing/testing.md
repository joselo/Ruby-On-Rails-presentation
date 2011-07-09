!SLIDE transition=scrollRight
# Testing #
!SLIDE transition=scrollRight
# Unit Testing #
# Integration Testing #
# Acceptance Testing #
!SLIDE transition=scrollRight
# Unit Testing #
## testunit ##
## rspec ##
!SLIDE bullets transition=scrollRight code smaller
## unit spec ##
    @@@ruby
    # bowling_spec.rb
    require 'bowling'

    describe Bowling, "#score" do
      it "returns 0 for all gutter game" do
        bowling = Bowling.new
        20.times { bowling.hit(0) }
        bowling.score.should == 0
      end
    end
***
    @@@ruby
    # bowling.rb
    class Bowling
      def hit(pins)
      end

      def score
       0
      end
    end
!SLIDE bullets transition=scrollRight code smaller
## request spec ##
    @@@ruby
    describe "widgets resource" do
      describe "GET index" do
        it "contains the widgets header" do
          get "/widgets/index"
          response.should have_selector("h1", :content => "Widgets")
        end
      end
    end
!SLIDE bullets transition=scrollRight code smaller
## routing spec ##
    @@@ruby
    describe "routing to profiles" do
      it "routes /profile/:username to profile#show for username" do
        { :get => "/profiles/jsmith" }.should route_to(
          :controller => "profiles",
          :action => "show",
          :username => "jsmith"
         )
      end

      it "does not expose a list of profiles" do
        { :get => "/profiles" }.should_not be_routable
      end
    end

!SLIDE transition=scrollRight
# Acceptance Testing #
## cucumber ##
!SLIDE bullets transition=scrollRight code smaller
    @@@cucumber
    Feature: Addition
      In order to avoid silly mistakes
      As a math idiot
      I want to be told the sum of two numbers

      Scenario Outline: Add two numbers
        Given I have entered <input_1> into the calculator
        And I have entered <input_2> into the calculator
        When I press <button>
        Then the result should be <output> on the screen

      Examples:
        | input_1 | input_2 | button | output |
        | 20      | 30      | add    | 50     |
        | 2       | 5       | add    | 7      |
        | 0       | 40      | add    | 40     |
