# SpeedDodge
import React, { useState, useEffect, useRef } from "react"; import { motion } from "framer-motion";

export default function CarRunningGame() { const [carX, setCarX] = useState(0); const [obstacleY, setObstacleY] = useState(-300); const [obstacleX, setObstacleX] = useState(0); const [score, setScore] = useState(0); const [gameOver, setGameOver] = useState(false);

useEffect(() => { const handleKeyDown = (e) => { if (e.key === "ArrowLeft") { setCarX((x) => Math.max(x - 25, -120)); } else if (e.key === "ArrowRight") { setCarX((x) => Math.min(x + 25, 120)); } }; window.addEventListener("keydown", handleKeyDown); return () => window.removeEventListener("keydown", handleKeyDown); }, []);

useEffect(() => { if (gameOver) return; const interval = setInterval(() => { setObstacleY((y) => { if (y >= 400) { setObstacleX(Math.floor(Math.random() * 5 - 2) * 60); setScore((s) => s + 1); return -300; } return y + 6; }); }, 20); return () => clearInterval(interval); }, [gameOver]);

useEffect(() => { if ( Math.abs(carX - obstacleX) < 50 && obstacleY > 220 && obstacleY < 300 ) { setGameOver(true); setTimeout(() => { alert(Game Over! Your Score: ${score}); window.location.reload(); }, 100); } }, [carX, obstacleX, obstacleY, score]);

return ( <div className="relative w-full h-screen bg-black overflow-hidden flex items-center justify-center"> <motion.div className="absolute bottom-10 w-16 h-32 bg-blue-500 rounded-md shadow-lg" animate={{ x: carX }} /> <motion.div className="absolute w-16 h-16 bg-red-500 rounded-md shadow" animate={{ y: obstacleY, x: obstacleX }} /> <div className="absolute top-4 left-4 text-white text-xl font-bold"> Score: {score} </div> </div> ); }

