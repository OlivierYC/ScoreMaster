# ScoreMaster
public static List<int> ScoreFrames (List<int> rolls) {
		List<int> frameList = new List<int> (); // The score of each of the 10 frames.

		// My code here.
		int frameTotal = 0; // The added score of the frame.	
		int turnCount = 0; // The number of turns in the list. Needed to count scores for strikes or spares.
		int framePosition = 0; // The position of the turn in the frame. Needed to count frames, regardless of turns bowled.
		bool frameComplete = false; // Sets if the frame is ready to be scored.
		bool holdingStrikeOrSpare = false; // Holds if the frame requires additional results to be scored.


		foreach (int roll in rolls){ // For each result in the number of turns, perform the following.


			if (roll < 0 || roll > 10) {throw new UnityException ("Invalid pin count.");} // A score of less than 0 or more than 10 in a turn throws an exception. Had to separate the single turn ROLL from the list ROLLS.

			turnCount ++; // Turns counted.
			framePosition ++; // Position of frames.
			frameTotal += roll; // Add up the results of each roll from the rolls.

		
			// Add frame score at end of game.
			if (framePosition == 21){
				return frameList;
			}

			// If the current frame is not ready and a single bowl is a 10, the logic for scoring a strike must follow.
			if (!frameComplete && roll == 10 && framePosition % 2 != 0){
				holdingStrikeOrSpare = true;

				if (rolls.Count >= turnCount + 2){

						frameTotal += rolls[turnCount + 1 - 1];
						frameTotal += rolls[turnCount + 2 - 1];
						framePosition ++; // Have to increase the turn count to account for the half-frame not bowled.
						frameComplete = true;

				}
			}

			// If the current frame is not ready, we are not holding for a Strike, and the total score for the frame is 10 ,then the logic for scoring a spare must follow.
			if (!frameComplete && !holdingStrikeOrSpare && frameTotal == 10){
				holdingStrikeOrSpare = true;

				if (rolls.Count >= turnCount + 1){ // When comparing the count and the next bowl, the count begins at 0 and the turns begin at 1, yet both add up to the same amount.
					frameTotal += rolls[turnCount + 1 - 1]; // When specifying the exact position in the list, the positional count begins at 0.
					frameComplete = true;
				}
			}

			//		int[] rolls = {1,2, 3,5, 5,5, 3,3, 7,1, 9,1, 6}; = {3, 8, 13, 6, 8, 16)

			// If the current frame is not ready and the end of the frame is determined by being even numbered.
			if (!frameComplete && !holdingStrikeOrSpare && framePosition % 2 == 0){
				frameComplete = true;
			}
		
			if (frameComplete){
				frameList.Add (frameTotal);
				frameTotal = 0; // Next frame total can now be counted.
				frameComplete = false; // Next frame can now be added together.
				holdingStrikeOrSpare = false;
			}
		}
		

		// My code ends here.

		return frameList;
	}
