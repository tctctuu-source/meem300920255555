import React, { useState, useEffect, useMemo } from 'react';
import { Search, Loader2, Trophy, Users, Award, Medal, Star } from 'lucide-react';
import { motion } from 'framer-motion';
import { supabase } from '../lib/supabase';
import { Result, Team } from '../types';
import PosterModal from '../components/PosterModal';

const Results: React.FC = () => {
  const [results, setResults] = useState<Result[]>([]);
  const [teams, setTeams] = useState<Team[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [searchTerm, setSearchTerm] = useState('');
  const [activeTab, setActiveTab] = useState<'program' | 'team'>('program');
  const [posterModalData, setPosterModalData] = useState<{ program: { event: string; category: string }; winners: Result[]; resultNumber: number } | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      
      try {
        const fetchPromises = [
          supabase.from('results').select('*').order('created_at', { ascending: false }),
          supabase.from('teams').select('*').order('points', { ascending: false })
        ];
        
        const [resultsResponse, teamsResponse] = await Promise.all(fetchPromises);

        if (resultsResponse.error) throw resultsResponse.error;
        setResults(resultsResponse.data || []);

        if (teamsResponse.error) throw teamsResponse.error;
        setTeams(teamsResponse.data || []);

      } catch (err: any) {
        setError("Could not fetch results. Please ensure your Supabase project is connected and running.");
        console.error("Error fetching results data:", err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  const uniquePrograms = useMemo(() => {
    if (!results) return [];
    const programMap = new Map<string, { event: string; category: string }>();
    results.forEach(result => {
      const key = `${result.event}-${result.category}`;
      if (!programMap.has(key)) {
        programMap.set(key, {
          event: result.event,
          category: result.category,
        });
      }
    });
    return Array.from(programMap.values()).sort((a, b) => a.event.localeCompare(b.event));
  }, [results]);

  const filteredPrograms = useMemo(() => {
    return uniquePrograms.filter(p =>
      p.event.toLowerCase().includes(searchTerm.toLowerCase()) ||
      p.category.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [uniquePrograms, searchTerm]);

  const filteredTeams = useMemo(() => {
    return teams.filter(t => t.name.toLowerCase().includes(searchTerm.toLowerCase()));
  }, [teams, searchTerm]);

  const rankColors = [
    { bg: 'bg-brand-mid-blue', gradient: 'bg-brand-mid-blue' },
    { bg: 'bg-brand-mid-blue', gradient: 'bg-brand-mid-blue' },
    { bg: 'bg-brand-mid-blue', gradient: 'bg-brand-mid-blue' },
    { bg: 'bg-brand-mid-blue', gradient: 'bg-brand-mid-blue' },
    { bg: 'bg-brand-mid-blue', gradient: 'bg-brand-mid-blue' },
  ];

  const programCardColors = [
    { topBg: 'bg-brand-mid-blue', circleBg: 'bg-brand-mid-blue', textColor: 'text-brand-dark-blue', icon: Trophy },
    { topBg: 'bg-brand-coral', circleBg: 'bg-brand-coral', textColor: 'text-brand-dark-blue', icon: Award },
    { topBg: 'bg-brand-dark-teal', circleBg: 'bg-brand-dark-teal', textColor: 'text-brand-dark-blue', icon: Medal },
    { topBg: 'bg-brand-coral/80', circleBg: 'bg-brand-coral/80', textColor: 'text-brand-dark-blue', icon: Star },
    { topBg: 'bg-brand-mid-blue/80', circleBg: 'bg-brand-mid-blue/80', textColor: 'text-brand-dark-blue', icon: Users },
  ];
  
  const handleProgramClick = (program: { event: string; category: string }, index: number) => {
    const programWinners = results.filter(r => r.event === program.event && r.category === program.category);
    setPosterModalData({ program, winners: programWinners, resultNumber: index + 1 });
  };

  return (
    <div className="py-8">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <motion.div
          initial={{ opacity: 0, y: 30 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.6 }}
          className="mb-8"
        >
          <h1 className="text-4xl font-bold text-ui-text-primary mb-2 font-fractual">Results</h1>
          <p className="text-ui-text-secondary">
            {loading ? 'Loading...' : `Published ${activeTab === 'program' ? filteredPrograms.length : filteredTeams.length} results`}
          </p>
        </motion.div>

        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.6, delay: 0.1 }}
          className="mb-8 space-y-4"
        >
          <div className="flex flex-col md:flex-row justify-between items-center gap-4">
            <div className="flex items-center bg-black/5 rounded-lg p-1">
              <button
                onClick={() => setActiveTab('program')}
                className={`px-6 py-2 rounded-md text-sm font-semibold transition-colors flex items-center gap-2 ${
                  activeTab === 'program' ? 'bg-brand-coral text-brand-dark-blue shadow' : 'text-ui-text-secondary hover:bg-black/5'
                }`}
              >
                <Trophy size={16} /> Program
              </button>
              <button
                onClick={() => setActiveTab('team')}
                className={`px-6 py-2 rounded-md text-sm font-semibold transition-colors flex items-center gap-2 ${
                  activeTab === 'team' ? 'bg-brand-coral text-brand-dark-blue shadow' : 'text-ui-text-secondary hover:bg-black/5'
                }`}
              >
                <Users size={16} /> Team
              </button>
            </div>
          </div>
          
          <div className="relative">
            <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-ui-text-secondary/50 w-5 h-5" />
            <input
              type="text"
              placeholder={`Search by ${activeTab === 'program' ? 'program or category' : 'team name'}...`}
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="w-full pl-10 pr-4 py-3 border border-black/10 rounded-lg focus:ring-2 focus:ring-brand-light-blue focus:border-transparent bg-ui-surface"
            />
          </div>
        </motion.div>

        {loading ? (
          <div className="flex justify-center items-center py-12"><Loader2 className="w-8 h-8 text-brand-mid-blue animate-spin" /></div>
        ) : error ? (
          <div className="text-center py-12">
            <div className="bg-rose-100 border border-rose-400 text-rose-700 px-4 py-3 rounded-lg inline-block">
              <h3 className="font-bold">Connection Error</h3>
              <p>{error}</p>
            </div>
          </div>
        ) : (
          <>
            {activeTab === 'program' && (
              <motion.div key="program-view" initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 0.6, delay: 0.2 }}>
                {filteredPrograms.length === 0 ? (
                  <div className="text-center py-12"><div className="text-ui-text-secondary/40 text-6xl mb-4">🔍</div><h3 className="text-xl font-semibold text-ui-text-secondary/80 mb-2">No Programs Found</h3><p className="text-ui-text-secondary/70">Try adjusting your search criteria.</p></div>
                ) : (
                  <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-x-4 gap-y-10">
                    {filteredPrograms.map((program, index) => {
                      const color = programCardColors[index % programCardColors.length];
                      const Icon = color.icon;
                      
                      return (
                        <motion.div
                          key={`${program.event}-${program.category}`}
                          initial={{ opacity: 0, y: 30 }}
                          animate={{ opacity: 1, y: 0 }}
                          transition={{ duration: 0.5, delay: (index % 24) * 0.05 }}
                          className="h-full"
                          onClick={() => handleProgramClick(program, index)}
                        >
                          <div className="relative bg-ui-surface rounded-2xl shadow-lg pt-8 pb-4 px-2 text-center h-full hover:shadow-xl hover:-translate-y-1 transition-all duration-300 cursor-pointer flex flex-col">
                            <div className={`absolute -top-5 left-1/2 -translate-x-1/2 w-28 h-12 ${color.topBg} rounded-full shadow-md`}>
                              <div className={`absolute -bottom-4 left-1/2 -translate-x-1/2 w-12 h-12 rounded-full flex items-center justify-center text-white font-bold text-lg shadow-lg border-4 border-ui-surface ${color.circleBg}`}>
                                {String(index + 1).padStart(2, '0')}
                              </div>
                            </div>
                            
                            <div className="flex-grow flex flex-col justify-center items-center pt-2">
                                <Icon className={`w-6 h-6 mx-auto mb-2 ${color.textColor}`} />
                                <h3 className={`font-bold text-sm text-ui-text-primary uppercase`}>{program.event}</h3>
                                <p className="text-xs text-ui-text-secondary mt-1">{program.category}</p>
                            </div>
                          </div>
                        </motion.div>
                      );
                    })}
                  </div>
                )}
              </motion.div>
            )}

            {activeTab === 'team' && (
              <motion.div key="team-view" initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 0.6 }}>
                {filteredTeams.length === 0 ? (
                  <div className="text-center py-12"><div className="text-ui-text-secondary/40 text-6xl mb-4">👥</div><h3 className="text-xl font-semibold text-ui-text-secondary/80 mb-2">No Teams Found</h3><p className="text-ui-text-secondary/70">Try adjusting your search criteria.</p></div>
                ) : (
                  <div className="space-y-5">
                    {filteredTeams.map((team, index) => {
                      const color = rankColors[index % rankColors.length];
                      return (
                        <motion.div
                          key={team.id}
                          initial={{ opacity: 0, x: -50 }}
                          animate={{ opacity: 1, x: 0 }}
                          transition={{ duration: 0.5, delay: index * 0.08 }}
                          className="bg-black/5 rounded-full p-1.5 shadow-inner"
                        >
                          <div className={`relative flex items-center h-16 rounded-full shadow-md ${color.bg}`}>
                            <div className={`z-10 flex-shrink-0 w-20 h-20 rounded-full flex items-center justify-center border-4 border-ui-text-light/50 shadow-lg bg-gradient-to-b ${color.gradient}`}>
                              <span className="text-ui-text-light font-bold text-3xl" style={{ textShadow: '0 2px 3px rgba(0,0,0,0.4)' }}>
                                {String(index + 1).padStart(2, '0')}
                              </span>
                            </div>
                            <div className="flex-grow h-full bg-ui-surface rounded-r-full flex justify-between items-center ml-[-45px] pl-[60px] pr-6">
                              <span className="text-lg md:text-xl font-bold text-ui-text-primary truncate">{team.name}</span>
                              <div className="text-right">
                                <span className="text-xl md:text-2xl font-bold text-ui-text-primary">{team.points}</span>
                                <span className="text-xs font-medium text-ui-text-secondary block">Points</span>
                              </div>
                            </div>
                          </div>
                        </motion.div>
                      );
                    })}
                  </div>
                )}
              </motion.div>
            )}
          </>
        )}
      </div>
      <PosterModal 
        isOpen={!!posterModalData}
        onClose={() => setPosterModalData(null)}
        data={posterModalData}
      />
    </div>
  );
};

export default Results;
