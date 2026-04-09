import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Checkbox } from "@/components/ui/checkbox";
import { motion } from "framer-motion";

export default function LifeGoalsApp() {
  const [goals, setGoals] = useState([
    { id: 1, text: "Faire le tour d'Europe en camion 🚐", done: false },
    { id: 2, text: "Voir les baleines 🐋", done: false },
  ]);

  const [showInput, setShowInput] = useState(false);
  const [newGoal, setNewGoal] = useState("");

  const addGoal = () => {
    if (!newGoal.trim()) return;
    setGoals([
      ...goals,
      { id: Date.now(), text: newGoal, done: false },
    ]);
    setNewGoal("");
    setShowInput(false);
  };

  const toggleGoal = (id) => {
    setGoals(
      goals.map((goal) =>
        goal.id === id ? { ...goal, done: !goal.done } : goal
      )
    );
  };

  return (
    <div className="relative min-h-screen bg-gradient-to-br from-yellow-300 via-orange-300 to-purple-400 p-6 overflow-hidden">

      <div className="max-w-2xl mx-auto relative z-10">

        

        <motion.h1
          initial={{ opacity: 0, y: -20 }}
          animate={{ opacity: 1, y: 0 }}
          className="text-3xl font-bold text-center text-white mb-6"
        >
          🌈 Mes Objectifs de Vie
        </motion.h1>

        <div className="space-y-3 pb-24">
          {goals.map((goal) => (
            <motion.div
              key={goal.id}
              initial={{ opacity: 0, scale: 0.9 }}
              animate={{ opacity: 1, scale: 1 }}
            >
              <Card className="rounded-2xl shadow-lg bg-white/90">
                <CardContent className="p-4 flex items-center justify-between">
                  <div className="flex items-center gap-3">
                    <Checkbox
                      checked={goal.done}
                      onCheckedChange={() => toggleGoal(goal.id)}
                    />
                    <span
                      className={`text-lg ${goal.done ? "line-through text-gray-400" : "text-gray-800"}`}
                    >
                      {goal.text}
                    </span>
                  </div>
                </CardContent>
              </Card>
            </motion.div>
          ))}
        </div>
      </div>

      {/* ➕ Floating button */}
      <Button
        onClick={() => setShowInput(true)}
        className="fixed bottom-6 right-6 w-16 h-16 rounded-full text-3xl shadow-xl bg-orange-400 hover:bg-orange-500 flex items-center justify-center"
      >
        <span className="text-purple-700">+</span>
      </Button>

      {/* ✨ Input modal */}
      {showInput && (
        <div className="fixed inset-0 bg-black/30 flex items-center justify-center z-20">
          <Card className="rounded-2xl shadow-xl w-80">
            <CardContent className="p-4 flex flex-col gap-3">
              <Input
                placeholder="Ton objectif... ✨"
                value={newGoal}
                onChange={(e) => setNewGoal(e.target.value)}
              />
              <div className="flex justify-end gap-2">
                <Button variant="ghost" onClick={() => setShowInput(false)}>
                  Annuler
                </Button>
                <Button onClick={addGoal}>Ajouter</Button>
              </div>
            </CardContent>
          </Card>
        </div>
      )}
    </div>
  );
}
