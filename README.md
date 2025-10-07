# TOEICbyDhana
I create good TOEIC mock questions for busy people.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOEIC S&W Practice App</title>
    <!-- Load Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load React, ReactDOM, and Babel -->
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    
    <style>
        /* Custom font and base styling */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .dark .shadow-2xl { box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.4); }
    </style>
</head>
<body class="bg-gray-50 dark:bg-gray-950">

    <div id="root">
        <!-- React component will be rendered here -->
    </div>

    <!-- The JSX code must be placed inside a script tag with type="text/babel" -->
    <script type="text/babel">
        // The previous 'import React, { useState, useCallback } from 'react';' was causing the error. 
        // We now rely on the globally loaded 'React' object and prefix all hooks.
        
        // Manual icon definitions to fix "require is not defined" error in single-file Babel setup.
        // These replace: import { Play, Mic, Edit, Check, Clock, Eye, ChevronLeft, ChevronRight } from 'lucide-react';
        
        const IconBase = ({ children, className = "", style = {}, ...props }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className} style={style} {...props}>
                {children}
            </svg>
        );

        const Play = (props) => (<IconBase {...props}><polygon points="5 3 19 12 5 21 5 3"></polygon></IconBase>);
        const Mic = (props) => (<IconBase {...props}><path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="22"></line></IconBase>);
        const Edit = (props) => (<IconBase {...props}><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></IconBase>);
        const Check = (props) => (<IconBase {...props}><polyline points="20 6 9 17 4 12"></polyline></IconBase>);
        const Clock = (props) => (<IconBase {...props}><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></IconBase>);
        const Eye = (props) => (<IconBase {...props}><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></IconBase>);
        const ChevronLeft = (props) => (<IconBase {...props}><polyline points="15 18 9 12 15 6"></polyline></IconBase>);
        const ChevronRight = (props) => (<IconBase {...props}><polyline points="9 18 15 12 9 6"></polyline></IconBase>);

        // --- MOCK TOEIC SPEAKING & WRITING PROMPTS ---
        const S_W_PROMPTS = [
          // Speaking Task: Describe a Picture (Question 2)
          {
            id: 1,
            type: 'Speaking: Describe a Picture',
            instructions: 'You have 30 seconds to prepare and 45 seconds to speak. Focus on describing key people, objects, and actions.',
            image: 'https://placehold.co/400x250/2563eb/ffffff?text=Busy+Office+Scene',
            promptText: 'Describe the picture below in as much detail as possible.',
            preparationTime: 30, // seconds
            responseTime: 45,    // seconds
            modelAnswer: 'The picture shows a modern, collaborative office setting. In the center, a woman in a blue shirt is actively talking on a phone headset, suggesting she is handling customer inquiries or calls. There are several colleagues nearby, focused on their desktop computers. The environment is well-lit, with natural light coming from the side. The overall impression is one of a busy, professional team environment utilizing technology to communicate and work efficiently. You can see file folders and various office supplies scattered across the desks.',
          },
          // Writing Task: Respond to a Written Request (Question 7)
          {
            id: 2,
            type: 'Writing: Respond to a Written Request (Email)',
            instructions: 'You have 10 minutes to write an email response. Ensure you address all three points in the task and maintain a professional, respectful tone.',
            image: null,
            promptText: 'Read the following email. Write an email to the sender, Ms. Lee, responding to her request.',
            passage: "Subject: Urgent: Request for meeting materials\n\nDear Team,\n\nCould you please send me the final sales figures from the last quarter's report by the end of the day? I need to review them before tomorrow's board meeting. Also, please confirm the exact time of the meeting with the new client, Synergy Corp. I seem to have misplaced the invitation.\n\nThanks,\nMs. Lee, Director of Sales",
            task: 'In your email, acknowledge the sales figures request, state that you will send the file within the hour, and confirm the client meeting is scheduled for 2:00 PM tomorrow.',
            responseTime: 600, // 10 minutes in seconds
            modelAnswer: "Subject: Re: Urgent: Request for meeting materials\n\nDear Ms. Lee,\n\nI have received your request for the final sales figures. I am compiling the necessary data now and will send you the complete report file within the next hour, well before the end of the business day.\n\nRegarding the meeting with Synergy Corp, I can confirm that it is scheduled for 2:00 PM tomorrow afternoon.\n\nPlease let me know if you require any additional information or have further questions.\n\nBest regards,\n[Your Name]",
          },
          // Speaking Task: Respond to Questions (Question 4 - Sample)
          {
            id: 3,
            type: 'Speaking: Respond to Questions',
            instructions: 'Imagine you are speaking to a research interviewer about public transportation. You have 15 seconds to answer this question.',
            image: null,
            promptText: 'How often do you use public transportation, and what is your preferred mode (bus, train, or subway)?',
            preparationTime: 0,
            responseTime: 15,
            modelAnswer: 'I typically use public transportation about three or four times a week. My preferred mode is definitely the subway. It is the fastest and most efficient way to travel across the city, especially during peak traffic hours, which saves me a lot of time on my daily commute.',
          },
        ];

        // --- Sub-Components ---

        // Timer component to simulate test conditions
        const Timer = ({ initialTime, isRunning, onTimeUp }) => {
          const [timeLeft, setTimeLeft] = React.useState(initialTime); // FIX: Prefixed useState

          React.useEffect(() => {
            setTimeLeft(initialTime);
          }, [initialTime]);

          React.useEffect(() => {
            let interval;
            if (isRunning && timeLeft > 0) {
              interval = setInterval(() => {
                setTimeLeft(prevTime => prevTime - 1);
              }, 1000);
            } else if (timeLeft === 0) {
              onTimeUp();
            }
            return () => clearInterval(interval);
          }, [isRunning, timeLeft, onTimeUp]);

          const minutes = Math.floor(timeLeft / 60);
          const seconds = timeLeft % 60;

          const timeColor = timeLeft <= 10 && timeLeft > 0 ? 'text-red-500' : 'text-indigo-600 dark:text-indigo-400';

          return (
            <div className="flex items-center space-x-2 p-2 bg-indigo-50 dark:bg-gray-800 rounded-lg shadow-inner">
              <Clock className={`h-5 w-5 ${timeColor}`} />
              <span className={`text-xl font-mono font-bold ${timeColor}`}>
                {String(minutes).padStart(2, '0')}:{String(seconds).padStart(2, '0')}
              </span>
              <span className="text-sm text-gray-500 dark:text-gray-400">
                Remaining
              </span>
            </div>
          );
        };

        // Main practice component
        const PracticeSection = ({ prompt, userResponse, setUserResponse }) => {
          const [stage, setStage] = React.useState('instruction'); // FIX: Prefixed useState
          const [showModel, setShowModel] = React.useState(false); // FIX: Prefixed useState

          const isSpeakingTask = prompt.type.includes('Speaking');

          // Unified time up handler
          const handleTimeUp = React.useCallback(() => { // FIX: Prefixed useCallback
            if (stage === 'preparation') {
              setStage('response');
            } else if (stage === 'response') {
              setStage('review');
            }
          }, [stage]);

          const handleStart = () => {
            if (prompt.preparationTime > 0 && isSpeakingTask) {
              setStage('preparation');
            } else {
              setStage('response');
            }
            setShowModel(false);
          };

          const currentInstructions = stage === 'preparation'
            ? `PREPARATION TIME: ${prompt.preparationTime} seconds. Plan your answer structure.`
            : stage === 'response'
              ? `RECORDING/WRITING TIME: ${prompt.responseTime / (isSpeakingTask ? 1 : 60)} ${isSpeakingTask ? 'seconds' : 'minutes'}. Start now!`
              : prompt.instructions;

          return (
            <div className="bg-white dark:bg-gray-900 p-6 sm:p-8 rounded-2xl shadow-2xl border border-gray-100 dark:border-gray-800">

              {/* Header and Instructions */}
              <div className="mb-6 pb-4 border-b border-gray-200 dark:border-gray-700">
                <h3 className="text-2xl font-extrabold text-indigo-600 dark:text-indigo-400 mb-1">
                  {prompt.type}
                </h3>
                <p className="text-sm text-gray-500 dark:text-gray-400 italic">{currentInstructions}</p>
              </div>

              {/* Prompt Area */}
              <div className="mb-6 p-4 bg-gray-50 dark:bg-gray-800 rounded-xl">
                <p className="font-semibold text-lg text-gray-800 dark:text-gray-200 mb-3">{prompt.promptText}</p>

                {prompt.image && (
                  <img src={prompt.image} alt="Practice Prompt Visual" className="w-full max-w-sm mx-auto rounded-lg shadow-md mb-4" onError={(e) => e.target.src = 'https://placehold.co/400x250/2563eb/ffffff?text=Image+Placeholder'} />
                )}

                {prompt.passage && (
                  <div className="p-3 bg-white dark:bg-gray-900 rounded-lg border border-gray-300 dark:border-gray-700 whitespace-pre-wrap text-sm font-mono text-gray-700 dark:text-gray-300">
                    {prompt.passage}
                  </div>
                )}

                {prompt.task && (
                  <div className="mt-3 p-3 bg-indigo-100 dark:bg-indigo-900/50 rounded-lg">
                    <p className="font-bold text-indigo-700 dark:text-indigo-300 flex items-center">
                      <Check className="h-4 w-4 mr-2" /> Task Requirements:
                    </p>
                    <p className="text-sm text-indigo-800 dark:text-indigo-200 mt-1">{prompt.task}</p>
                  </div>
                )}
              </div>

              {/* Action / Timer Area */}
              <div className="flex justify-between items-center mb-6">
                {stage === 'instruction' && (
                  <button
                    onClick={handleStart}
                    className="flex items-center bg-green-600 text-white font-bold py-3 px-6 rounded-xl transition-transform hover:scale-[1.02] shadow-lg shadow-green-500/50"
                  >
                    <Play className="h-5 w-5 mr-2" />
                    Start Practice
                  </button>
                )}

                {(stage === 'preparation' || stage === 'response') && (
                  <Timer
                    initialTime={stage === 'preparation' ? prompt.preparationTime : prompt.responseTime}
                    isRunning={true}
                    onTimeUp={handleTimeUp}
                  />
                )}

                {stage === 'response' && isSpeakingTask && (
                  <div className="text-sm text-gray-500 dark:text-gray-400 flex items-center">
                    <Mic className="h-4 w-4 mr-1" />
                    *Please record your voice response locally (e.g., using your phone or a separate app).*
                  </div>
                )}

                {(stage === 'response' || stage === 'preparation') && (
                  <button
                    onClick={() => handleTimeUp()} // Allows user to skip time and submit/move to next stage
                    className="bg-gray-300 dark:bg-gray-700 text-gray-800 dark:text-gray-200 font-semibold py-3 px-5 rounded-full transition-transform hover:scale-105"
                  >
                    {stage === 'preparation' ? 'Skip Prep' : 'Finish Early'}
                  </button>
                )}

                {stage === 'review' && (
                  <button
                    onClick={() => setShowModel(prev => !prev)}
                    className="flex items-center bg-indigo-600 text-white font-bold py-3 px-6 rounded-xl transition-transform hover:scale-[1.02] shadow-lg shadow-indigo-500/50"
                  >
                    <Eye className="h-5 w-5 mr-2" />
                    {showModel ? 'Hide Model Answer' : 'Show Model Answer'}
                  </button>
                )}
              </div>

              {/* Response Area - Visible during response and review stages */}
              {(stage === 'response' || stage === 'review') && (
                <>
                  <h4 className="text-xl font-bold mt-4 mb-2 text-gray-700 dark:text-gray-300 flex items-center">
                    <Edit className="h-5 w-5 mr-2" />
                    Your Typed Response (Practice/Review):
                  </h4>
                  <textarea
                    value={userResponse}
                    onChange={(e) => setUserResponse(e.target.value)}
                    disabled={stage === 'review'}
                    rows={isSpeakingTask ? 6 : 10}
                    className="w-full p-4 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-gray-200 focus:ring-indigo-500 focus:border-indigo-500 shadow-inner resize-none"
                    placeholder={isSpeakingTask ? "Type out your planned spoken response for self-correction..." : "Type your email/essay response here..."}
                  />
                </>
              )}

              {/* Model Answer Area */}
              {stage === 'review' && showModel && (
                <div className="mt-8 p-6 bg-green-50 dark:bg-green-900/50 rounded-2xl border-l-4 border-green-500 shadow-md">
                  <h4 className="text-xl font-bold mb-2 text-green-800 dark:text-green-300 flex items-center">
                    <Check className="h-5 w-5 mr-2" />
                    High-Scoring Model Answer:
                  </h4>
                  <div className="whitespace-pre-wrap text-gray-700 dark:text-green-200">
                    {prompt.modelAnswer}
                  </div>
                  <p className="mt-4 text-sm italic text-green-600 dark:text-green-400">
                    Compare your response for structure, grammar, and comprehensive task coverage.
                  </p>
                </div>
              )}
            </div>
          );
        };

        // --- Main App Component ---
        const App = () => {
          const [currentPromptIndex, setCurrentPromptIndex] = React.useState(0); // FIX: Prefixed useState
          const [userResponses, setUserResponses] = React.useState({}); // FIX: Prefixed useState

          const currentPrompt = S_W_PROMPTS[currentPromptIndex];
          const totalPrompts = S_W_PROMPTS.length;

          // Handler for updating the text area value for the current prompt
          const handleUserResponseChange = React.useCallback((e) => { // FIX: Prefixed useCallback
            setUserResponses(prev => ({
              ...prev,
              [currentPrompt.id]: e.target.value,
            }));
          }, [currentPrompt.id]);

          const handleNext = () => {
            if (currentPromptIndex < totalPrompts - 1) {
              setCurrentPromptIndex(prev => prev + 1);
            }
          };

          const handlePrev = () => {
            if (currentPromptIndex > 0) {
              setCurrentPromptIndex(prev => prev - 1);
            }
          };

          return (
            <div className="min-h-screen bg-gray-50 dark:bg-gray-950 font-sans p-4 sm:p-8">

              {/* Main Container */}
              <div className="max-w-4xl mx-auto py-4">
                <header className="text-center mb-10">
                  <h1 className="text-4xl sm:text-5xl font-extrabold text-gray-900 dark:text-white leading-tight">
                    TOEIC Speaking & Writing <span className="text-indigo-600 dark:text-indigo-400">Practice</span>
                  </h1>
                  <p className="mt-2 text-lg text-gray-600 dark:text-gray-400">
                    Develop fluency and structure for the production sections.
                  </p>
                </header>

                <main>
                  <PracticeSection
                    prompt={currentPrompt}
                    userResponse={userResponses[currentPrompt.id] || ''}
                    setUserResponse={handleUserResponseChange}
                    key={currentPrompt.id} // Key ensures the component resets state (like 'stage') when navigating
                  />
                </main>

                {/* Navigation Controls */}
                <div className="flex justify-between items-center mt-8 p-4 bg-white dark:bg-gray-900 rounded-2xl shadow-xl">
                  <button
                    onClick={handlePrev}
                    disabled={currentPromptIndex === 0}
                    className="flex items-center bg-gray-200 dark:bg-gray-700 text-gray-800 dark:text-gray-200 font-semibold py-3 px-5 rounded-full transition-transform hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed"
                  >
                    <ChevronLeft className="h-5 w-5 mr-1" />
                    Previous Prompt
                  </button>
                  <span className="text-lg font-bold text-indigo-600 dark:text-indigo-400">
                    Prompt {currentPromptIndex + 1} / {totalPrompts}
                  </span>
                  <button
                    onClick={handleNext}
                    disabled={currentPromptIndex === totalPrompts - 1}
                    className="flex items-center bg-indigo-600 text-white font-semibold py-3 px-5 rounded-full transition-transform hover:scale-105 shadow-md shadow-indigo-500/50 disabled:opacity-50 disabled:cursor-not-allowed"
                  >
                    Next Prompt
                    <ChevronRight className="h-5 w-5 ml-1" />
                  </button>
                </div>
              </div>
            </div>
          );
        };

        // Render the App component into the #root div
        ReactDOM.createRoot(document.getElementById('root')).render(<App />);
    </script>
</body>
</html>

 
    
             
              

       
