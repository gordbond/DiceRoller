
<!DOCTYPE html>
<html>

<head>
  <title>ReactJS Container Component</title>
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
  <link href="https://fonts.googleapis.com/css?family=Permanent+Marker&display=swap" rel="stylesheet">

  <style>
  .myclass {
    color: red;
  }
  #title{
    width: 700px; margin: 50px auto; 
  }
  #counters{
    display: flex; margin: auto; max-width: 1000px; flex-wrap: wrap; 
    
  }
  img{
    width: 100%;
  }
  #numOfDiceCont{
    width: 30%;  margin: auto; 
  }

  #numOfSidesCont{
    width: 30%; margin: auto; 
  }
 
  li{display: inline;}

  ul{text-align: center; display: flex; justify-content: center; flex-wrap: wrap;}

  .label{
    width:150px; margin: auto; 
  }
  #rollButton{
    width: 150px; margin: auto;
    cursor: pointer;
    flex-wrap: wrap;
  }

  #rollButton:active{

    transform: translateY(5px);
    /* transform: translateX(-2px); */
  }

  .die{
    width: 40px;
    height: 40px;
    background-color:rgba(199,50,50,0.75);

    text-decoration: none;
    text-align: center;
    list-style: none;
    padding: 20px 20px;
    margin: 10px 20px;
    border-radius: 5px;
color:rgb(255, 255, 255);
    /* -webkit-box-shadow: inset 0px 0px 50px 11px rgba(224,11,224,0.64);
    -moz-box-shadow: inset 0px 0px 50px 11px rgba(224,11,224,0.64);
    box-shadow: inset 0px 0px 50px 11px rgba(224,11,224,0.64); */

    -webkit-box-shadow: -1px 13px 5px -6px rgba(0,0,0,0.5);
    -moz-box-shadow: -1px 13px 5px -6px rgba(0,0,0,0.5);
    box-shadow: -1px 13px 5px -6px rgba(0,0,0,0.5);
  

  }

  .die div{
    display: table-cell;
    /* vertical-align: middle; */
    width: 40px;
    height: 40px;
    font-size: 1.6em;
    font-family: 'Permanent Marker', cursive;
    

  }
  
  .counter{
    font-family: 'Permanent Marker', cursive;
    font-size: 2.5em;
    color: rgba(199,50,50, 0.9);
    width: 200px;
    margin:auto;
    padding: 5px 10px;
    text-align: center;
  }

  #counters button{
    border: 2px black solid;
    border-radius: 5px;
    padding: 0 18px;
    font-family: 'Permanent Marker', cursive;
    font-size: 1em;
    cursor: pointer;
    margin: 10px;
    text-align: center;
  }

  #sum{
    font-family: 'Permanent Marker', cursive;
  }

  #counters button:focus {outline:0;}

  #bottomCont{
    text-align: center;
  }

  </style>
</head>

<body>

  <div id="mainContainer">
    
  </div>
    
      



  <script type="text/babel">




   /* 
    Main container

    Maintains the state for the dice roller
    - State includes:
      - number of sides 
      - number of dice
    Renders the dice presentation and counter presentation components
   */

//---------------------------MAIN: Passes state to counters and dice------------------------//
   class MainContainer extends React.Component {

    //Constructor for MainContainer component class
     constructor(props) 
     {
       super(props);
       this.state = 
       {
         numOfDice: 0,
         numOfSides: 0,
         dice: [],
         sum:0
       }
       //this.setSum.bind(this);
       this.diceRoll.bind(this);
     }

     componentDidMount() {

       this.setState(
         {
          numOfDice: 1,
          numOfSides: 6,
         }
       );
       
     }
     //counter for id
     previd = 1;
     /* 
        Controls the logic for the counter being incremented
        - not included in the assignment but I placed max limits on dice and sides
        - Dice cannot exceed 10
        - Number of sides cannot exceed 20
     */
     counterIncremented(id){
       console.log("ID: "+id);
        if(id === "die"){
          if(this.state.numOfDice < 10){
              this.setState(
            {
                  numOfDice: this.state.numOfDice+ 1
            }
              );
          }
        }else{
          if(this.state.numOfSides < 20){
              this.setState(
            {
              numOfSides: this.state.numOfSides +  1
            });
          }
        }
     }
      /* 
        Controls the logic for the counter being decremented
        - Dice cannot be less than 1
        - Number of sides cannot be less than 2
     */
     counterDecremented(id){
        console.log("ID: "+id);
        if(id === "die"){
          if(this.state.numOfDice > 1){
              this.setState(
            {
              numOfDice: this.state.numOfDice - 1
            }
              );
          }
        }else{
          if(this.state.numOfSides > 2){

              this.setState(
            {
              numOfSides: this.state.numOfSides -  1
            });
          }
        }
     }
     
     //Rolls random number between 1 and the number of sides
     //creates number of dice equal to the numOfDice with a new random number
     diceRoll(){
       console.log('Clicked in diceRoll');
        this.setState({dice:[]});
        for(let i = 0; i < this.state.numOfDice; i++){
          let randNum = Math.floor((Math.random() * Math.floor(this.state.numOfSides - 1)) + 1) ;
          this.setState( previousState => {
            return {
              dice:[
                {
                  id: this.previd += 1,
                  dieVal: randNum
                },
                ...previousState.dice
              ]
            }
          });
         
        }
       
     }

     
     //Render Counter Presentation and Dice Presentation components
     render() {
       return (
        <div>
          < CounterPresentation 
            numOfSides={this.state.numOfSides} 
            numOfDice={this.state.numOfDice}
            counterIncremented={this.counterIncremented.bind(this)}
            counterDecremented = {this.counterDecremented.bind(this)}
            diceRoll={this.diceRoll.bind(this)}
          />
          < DicePresentation 
            dice={this.state.dice}
          />
        </div>
       );
     }
   };

//-----------------------DICE: Component controlling the presentation of the dice elements-----------------------//
   //Renders all dice in state to html
   class DicePresentation extends React.Component {


    renderDice({id, dieVal}){
      return(
        <li
          className="die" 
          key={id}
        ><div>
          {dieVal}
          </div>
        </li>
      );
    }

  
     render() {
       return (
         <div id="bottomCont">
            <h1 id="sum">Sum: {this.props.dice.reduce(function(tot, die){
              return tot + die.dieVal
            },0)}</h1>
            <ul>
              {this.props.dice.map(this.renderDice.bind(this))}
            </ul>
         </div>
       );
     }
   };

//--------------------------COUNTERS: Component controlling the presentation of the counters-------------------//
   class CounterPresentation extends React.Component{

      //Handles the incrementing of both sides and number of dice
      handleIncrement(e) {
        e.preventDefault();
        this.props.counterIncremented(e.target.title)
      }

      //Handles the decrementing of both sides and number of dice
      handleDecrement(e){
        e.preventDefault();
        this.props.counterDecremented(e.target.title)
      }

      //Returns the HTML for the buttons and counters
      render() {
        return(
          <div>
            <div id="title" ><img src={"img/title3.jpg"} /></div>

              <div id="counters" >

                <div id="numOfDiceCont">
                  <div id="dieCounter">
                    <div id="numOfDiceLabel" className="label"><img src={"img/numOfDice.jpg"} /></div>
                    <div id="dieVal" className="counter">{this.props.numOfDice}</div>
                    <div className="counter"><button title="die" onClick={this.handleIncrement.bind(this)}>+</button><button title="die" onClick={this.handleDecrement.bind(this)}>-</button></div>
                  </div>
                </div>
               
                <div id="rollButton" ><a onClick={this.props.diceRoll}><img src={"img/button.jpg"}/></a></div>

                <div id="numOfSidesCont" >
                  <div id="sideCounter">
                    <div id="numOfSidesLabel" className="label"><img src={"img/numOfSides.jpg"} /></div>
                    <div className="counter">{this.props.numOfSides}</div>
                    <div className="counter"><button title="side" onClick={this.handleIncrement.bind(this)}>+</button><button title="side" onClick={this.handleDecrement.bind(this)}>-</button></div> 
                  </div>
                </div>
              </div>
            </div>
        );
      }

    };




  //Renders the page
   ReactDOM.render(
     <MainContainer />, 
     document.querySelector("#mainContainer")
   );




  </script>



</body>

</html>
