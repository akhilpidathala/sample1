#hello world
import React, { useState, useEffect, useRef } from 'react';
import { Play, RotateCcw, Download, Eye, EyeOff } from 'lucide-react';

const CodeEditor = () => {
  const [html, setHtml] = useState(`<div class="container">
  <h1>Hello CodePen!</h1>
  <p>Start coding and see the magic happen ✨</p>
  <button onclick="changeColor()">Click me!</button>
</div>`);
  
  const [css, setCss] = useState(`.container {
  text-align: center;
  padding: 2rem;
  font-family: 'Arial', sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
}

button {
  padding: 12px 24px;
  font-size: 1rem;
  background: #ff6b6b;
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255,107,107,0.3);
}

button:hover {
  background: #ff5252;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255,107,107,0.4);
}`);
  
  const [js, setJs] = useState(`function changeColor() {
  const colors = [
    'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
    'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)',
    'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)',
    'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)',
    'linear-gradient(135deg, #fa709a 0%, #fee140 100%)'
  ];
  
  const randomColor = colors[Math.floor(Math.random() * colors.length)];
  document.querySelector('.container').style.background = randomColor;
}

// Add some interactive features
document.addEventListener('DOMContentLoaded', function() {
  console.log('CodePen clone loaded successfully!');
  
  // Add keyboard listener for demo
  document.addEventListener('keydown', function(e) {
    if (e.key === 'Enter' && e.ctrlKey) {
      changeColor();
    }
  });
});`);

  const [compiledOutput, setCompiledOutput] = useState('');
  const [isPreviewVisible, setIsPreviewVisible] = useState(true);
  const [activeTab, setActiveTab] = useState('html');
  const iframeRef = useRef(null);

  const compileCode = () => {
    const compiledHTML = `
      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>CodePen Output</title>
        <style>
          body { margin: 0; padding: 0; }
          ${css}
        </style>
      </head>
      <body>
        ${html}
        <script>
          try {
            ${js}
          } catch (error) {
            console.error('JavaScript Error:', error);
            document.body.innerHTML += '<div style="background: #ff4444; color: white; padding: 10px; font-family: monospace;">JavaScript Error: ' + error.message + '</div>';
          }
        </script>
      </body>
      </html>
    `;
    setCompiledOutput(compiledHTML);
  };

  const resetCode = () => {
    setHtml(`<div class="container">
  <h1>Hello CodePen!</h1>
  <p>Start coding and see the magic happen ✨</p>
  <button onclick="changeColor()">Click me!</button>
</div>`);
    setCss(`.container {
  text-align: center;
  padding: 2rem;
  font-family: 'Arial', sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
}

button {
  padding: 12px 24px;
  font-size: 1rem;
  background: #ff6b6b;
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255,107,107,0.3);
}

button:hover {
  background: #ff5252;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255,107,107,0.4);
}`);
    setJs(`function changeColor() {
  const colors = [
    'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
    'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)',
    'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)',
    'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)',
    'linear-gradient(135deg, #fa709a 0%, #fee140 100%)'
  ];
  
  const randomColor = colors[Math.floor(Math.random() * colors.length)];
  document.querySelector('.container').style.background = randomColor;
}

// Add some interactive features
document.addEventListener('DOMContentLoaded', function() {
  console.log('CodePen clone loaded successfully!');
  
  // Add keyboard listener for demo
  document.addEventListener('keydown', function(e) {
    if (e.key === 'Enter' && e.ctrlKey) {
      changeColor();
    }
  });
});`);
  };

  const downloadCode = () => {
    const compiledHTML = `<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CodePen Export</title>
  <style>
    body { margin: 0; padding: 0; }
    ${css}
  </style>
</head>
<body>
  ${html}
  <script>
    ${js}
  </script>
</body>
</html>`;

    const blob = new Blob([compiledHTML], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'codepen-export.html';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  };

  // Auto-compile on code changes with debounce
  useEffect(() => {
    const timer = setTimeout(() => {
      compileCode();
    }, 500);

    return () => clearTimeout(timer);
  }, [html, css, js]);

  // Update iframe content
  useEffect(() => {
    if (iframeRef.current && compiledOutput) {
      const iframe = iframeRef.current;
      iframe.srcdoc = compiledOutput;
    }
  }, [compiledOutput]);

  const tabs = [
    { id: 'html', label: 'HTML', color: 'bg-orange-500' },
    { id: 'css', label: 'CSS', color: 'bg-blue-500' },
    { id: 'js', label: 'JS', color: 'bg-yellow-500' }
  ];

  const getCurrentCode = () => {
    switch (activeTab) {
      case 'html': return html;
      case 'css': return css;
      case 'js': return js;
      default: return '';
    }
  };

  const setCurrentCode = (value) => {
    switch (activeTab) {
      case 'html': setHtml(value); break;
      case 'css': setCss(value); break;
      case 'js': setJs(value); break;
    }
  };

  return (
    <div className="min-h-screen bg-gray-900 text-white flex flex-col">
      {/* Header */}
      <div className="bg-gray-800 p-4 border-b border-gray-700">
        <div className="flex justify-between items-center">
          <h1 className="text-2xl font-bold bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-transparent">
            CodePen Clone
          </h1>
          <div className="flex gap-2">
            <button
              onClick={compileCode}
              className="flex items-center gap-2 px-4 py-2 bg-green-600 hover:bg-green-700 rounded-lg transition-colors"
            >
              <Play size={16} />
              Run
            </button>
            <button
              onClick={resetCode}
              className="flex items-center gap-2 px-4 py-2 bg-gray-600 hover:bg-gray-700 rounded-lg transition-colors"
            >
              <RotateCcw size={16} />
              Reset
            </button>
            <button
              onClick={downloadCode}
              className="flex items-center gap-2 px-4 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg transition-colors"
            >
              <Download size={16} />
              Export
            </button>
            <button
              onClick={() => setIsPreviewVisible(!isPreviewVisible)}
              className="flex items-center gap-2 px-4 py-2 bg-purple-600 hover:bg-purple-700 rounded-lg transition-colors"
            >
              {isPreviewVisible ? <EyeOff size={16} /> : <Eye size={16} />}
              {isPreviewVisible ? 'Hide' : 'Show'} Preview
            </button>
          </div>
        </div>
      </div>

      {/* Main Content */}
      <div className="flex-1 flex">
        {/* Code Editor */}
        <div className={`${isPreviewVisible ? 'w-1/2' : 'w-full'} flex flex-col border-r border-gray-700`}>
          {/* Tabs */}
          <div className="flex bg-gray-800">
            {tabs.map((tab) => (
              <button
                key={tab.id}
                onClick={() => setActiveTab(tab.id)}
                className={`px-6 py-3 font-semibold transition-colors border-b-2 ${
                  activeTab === tab.id
                    ? `${tab.color} border-current text-white`
                    : 'bg-gray-700 border-transparent text-gray-300 hover:text-white hover:bg-gray-600'
                }`}
              >
                {tab.label}
              </button>
            ))}
          </div>
          
          {/* Code Area */}
          <div className="flex-1 p-4 bg-gray-900">
            <textarea
              value={getCurrentCode()}
              onChange={(e) => setCurrentCode(e.target.value)}
              className="w-full h-full bg-gray-800 text-green-400 font-mono text-sm p-4 border border-gray-600 rounded-lg resize-none focus:outline-none focus:ring-2 focus:ring-purple-500"
              placeholder={`Enter your ${activeTab.toUpperCase()} code here...`}
              spellCheck={false}
            />
          </div>
        </div>

        {/* Preview */}
        {isPreviewVisible && (
          <div className="w-1/2 flex flex-col">
            <div className="bg-gray-800 px-4 py-3 border-b border-gray-700">
              <h2 className="font-semibold text-gray-300">Preview</h2>
            </div>
            <div className="flex-1 bg-white">
              <iframe
                ref={iframeRef}
                className="w-full h-full border-none"
                title="Code Preview"
                sandbox="allow-scripts allow-same-origin"
              />
            </div>
          </div>
        )}
      </div>

      {/* Footer */}
      <div className="bg-gray-800 p-3 border-t border-gray-700 text-center text-sm text-gray-400">
        Press Ctrl+Enter in the preview to trigger color change • Real-time compilation enabled
      </div>
    </div>
  );
};

export default CodeEditor;
