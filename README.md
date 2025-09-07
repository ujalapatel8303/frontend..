# frontend..
import React, { useState } from 'react';
import { Zap } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { useToast } from '@/hooks/use-toast';
import { Hero } from '@/components/Hero';
import { ResumeUpload } from '@/components/ResumeUpload';
import { JobRoleInput } from '@/components/JobRoleInput';
import { AnalysisResults } from '@/components/AnalysisResults';

const Index = () => {
  const [resume, setResume] = useState<{ content: string; type: 'file' | 'text'; fileName?: string } | null>(null);
  const [jobRole, setJobRole] = useState('');
  const [jobDescription, setJobDescription] = useState('');
  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [hasResults, setHasResults] = useState(false);
  const { toast } = useToast();

  const handleAnalyze = async () => {
    if (!resume?.content.trim()) {
      toast({
        title: "Resume Required",
        description: "Please upload your resume or paste your resume text.",
        variant: "destructive",
      });
      return;
    }

    if (!jobRole.trim()) {
      toast({
        title: "Job Role Required",
        description: "Please specify the target job role.",
        variant: "destructive",
      });
      return;
    }

    setIsAnalyzing(true);
    
    // Simulate API call delay
    setTimeout(() => {
      setIsAnalyzing(false);
      setHasResults(true);
      toast({
        title: "Analysis Complete",
        description: "Your resume has been analyzed successfully!",
      });
    }, 3000);
  };

  const canAnalyze = resume?.content.trim() && jobRole.trim();

  return (
    <div className="min-h-screen bg-background">
      <Hero />
      
      <div className="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-16">
        <div className="space-y-8">
          {/* Upload Section */}
          <div className="space-y-6">
            <div className="text-center space-y-2">
              <h2 className="text-3xl font-bold text-foreground">Get Started</h2>
              <p className="text-muted-foreground">Upload your resume and specify your target role</p>
            </div>
            
            <ResumeUpload 
              onResumeChange={setResume}
              resume={resume}
            />
            
            <JobRoleInput
              jobRole={jobRole}
              onJobRoleChange={setJobRole}
              jobDescription={jobDescription}
              onJobDescriptionChange={setJobDescription}
            />
            
            <div className="text-center">
              <Button
                onClick={handleAnalyze}
                disabled={!canAnalyze || isAnalyzing}
                size="lg"
                className="px-8"
              >
                <Zap className="w-5 h-5 mr-2" />
                {isAnalyzing ? 'Analyzing...' : 'Analyze Resume'}
              </Button>
            </div>
          </div>

          {/* Results Section */}
          {(isAnalyzing || hasResults) && (
            <div className="space-y-6">
              <div className="text-center space-y-2">
                <h2 className="text-3xl font-bold text-foreground">Analysis Results</h2>
                <p className="text-muted-foreground">AI-powered insights for your resume</p>
              </div>
              
              <AnalysisResults isAnalyzing={isAnalyzing} />
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default Index;
