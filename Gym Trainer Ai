import { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Progress } from "@/components/ui/progress";
import { motion } from "framer-motion";
import { Line } from "react-chartjs-2";
import { Chart as ChartJS, LineElement, CategoryScale, LinearScale, PointElement } from "chart.js";
import useSound from "use-sound";

ChartJS.register(LineElement, CategoryScale, LinearScale, PointElement);

export default function GymTrainerAI() {
  const [goal, setGoal] = useState(null);
  const [progress, setProgress] = useState(0);
  const [workout, setWorkout] = useState(null);
  const [streak, setStreak] = useState(0);
  const [tips, setTips] = useState("");
  const [leaderboard, setLeaderboard] = useState([]);
  const [dietPlan, setDietPlan] = useState("");
  const [reminder, setReminder] = useState(false);
  const [progressHistory, setProgressHistory] = useState([]);

  const [playClick] = useSound("/sounds/click.mp3");
  const [playSuccess] = useSound("/sounds/success.mp3");

  const goals = {
    "Muscle Gain": ["Bench Press - 4 sets", "Squats - 4 sets", "Deadlifts - 3 sets"],
    "Fat Loss": ["HIIT - 20 min", "Jump Rope - 10 min", "Burpees - 3 sets"],
    "Strength": ["Heavy Deadlifts - 5 sets", "Pull-Ups - 3 sets", "Overhead Press - 4 sets"],
  };

  const dietPlans = {
    "Muscle Gain": "High protein diet: Chicken, Eggs, Lentils, Nuts, Whole Grains.",
    "Fat Loss": "Caloric deficit diet: Vegetables, Lean Meat, Low Carbs, Green Tea.",
    "Strength": "Balanced diet: Proteins, Carbs, Healthy Fats, Hydration Focus."
  };

  const motivationalTips = [
    "Stay consistent and trust the process!",
    "Focus on form, not just weight!",
    "Fuel your body with proper nutrition!",
    "Rest is just as important as training!",
    "Push yourself, but listen to your body!"
  ];

  useEffect(() => {
    if (reminder) {
      alert("Time for your workout! Let's get moving! 💪");
    }
  }, [reminder]);

  const handleSelectGoal = (selectedGoal) => {
    playClick();
    setGoal(selectedGoal);
    setWorkout(goals[selectedGoal]);
    setProgress(0);
    setStreak(0);
    setTips(motivationalTips[Math.floor(Math.random() * motivationalTips.length)]);
    setDietPlan(dietPlans[selectedGoal]);
  };

  const completeWorkout = () => {
    playSuccess();
    setProgress((prev) => Math.min(prev + 20, 100));
    setStreak((prev) => prev + 1);
    setTips(motivationalTips[Math.floor(Math.random() * motivationalTips.length)]);
    setProgressHistory([...progressHistory, progress + 20]);
    updateLeaderboard();
  };

  const updateLeaderboard = () => {
    setLeaderboard((prev) => [...prev, { streak }].sort((a, b) => b.streak - a.streak));
  };

  return (
    <motion.div 
      initial={{ opacity: 0, y: -20 }} 
      animate={{ opacity: 1, y: 0 }} 
      transition={{ duration: 0.5 }}
      className="p-6 max-w-lg mx-auto text-center"
    >
      <h1 className="text-2xl font-bold mb-4">🏋️ Gym Trainer AI</h1>
      <Button onClick={() => setReminder(true)} className="mb-4">Set Daily Workout Reminder</Button>
      {!goal ? (
        <motion.div initial={{ scale: 0.8 }} animate={{ scale: 1 }} transition={{ duration: 0.3 }}>
          <h2 className="text-lg font-semibold mb-2">Choose Your Goal</h2>
          <div className="flex flex-wrap justify-center gap-4">
            {Object.keys(goals).map((g) => (
              <Button key={g} onClick={() => handleSelectGoal(g)}>{g}</Button>
            ))}
          </div>
        </motion.div>
      ) : (
        <Card className="p-4 shadow-lg">
          <CardContent>
            <h2 className="text-xl font-semibold">{goal} Workout</h2>
            <ul className="text-left mt-2">
              {workout.map((exercise, index) => (
                <li key={index} className="py-1">✅ {exercise}</li>
              ))}
            </ul>
            <p className="mt-4 italic">🍽️ Diet Plan: {dietPlan}</p>
            <Progress value={progress} className="mt-4" />
            <p className="mt-2">Progress: {progress}%</p>
            <p className="mt-2 font-semibold text-green-600">🔥 Streak: {streak} days</p>
            <p className="mt-2 italic">💡 {tips}</p>
            <Line data={{ labels: progressHistory.map((_, i) => `Day ${i+1}`), datasets: [{ label: 'Progress', data: progressHistory, borderColor: 'blue', fill: false }] }} />
            {progress < 100 && (
              <Button onClick={completeWorkout} className="mt-3">Complete Workout</Button>
            )}
          </CardContent>
        </Card>
      )}
    </motion.div>
  );
}
