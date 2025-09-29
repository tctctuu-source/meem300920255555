import React from 'react';
import { Result, PosterStyles } from '../../../types';
import { Trophy } from 'lucide-react';

interface PosterLayoutProps {
  program: { event: string; category: string };
  winners: Result[];
  styles: PosterStyles;
}

const StyledText: React.FC<{ style: any; children: React.ReactNode; className?: string }> = ({ style, children, className }) => {
  if (!style) return <div className={`text-red-500 ${className}`}>Style not defined</div>;
  
  const cssProperties: React.CSSProperties = {
    position: 'absolute',
    top: `${style.y}%`,
    left: `${style.x}%`,
    transform: 'translate(-50%, -50%)',
    width: '90%',
    fontSize: `${style.size}px`,
    color: style.color,
    textAlign: style.align,
    fontWeight: style.weight,
    fontFamily: `'${style.font}', sans-serif`,
    lineHeight: 1.2,
  };

  return <div style={cssProperties} className={className}>{children}</div>;
};

const PosterLayout5: React.FC<PosterLayoutProps> = ({ program, winners, styles }) => {
  const winner1 = winners.find(w => w.position === 1);
  const otherWinners = winners.filter(w => w.position > 1).sort((a,b) => a.position - b.position);

  return (
    <div className="w-full h-full bg-brand-dark-teal text-ui-text-light font-sans p-8 relative">
      <StyledText style={styles.headerTitle} className="uppercase tracking-tighter">RESULTS</StyledText>
      <StyledText style={styles.orgName}>MUHIMMATH</StyledText>
      <StyledText style={styles.orgSubName}>SSF DAAWA SECTOR</StyledText>
      
      <StyledText style={styles.programName} className="uppercase">{program.event}</StyledText>
      <StyledText style={styles.category}>{program.category}</StyledText>
      
      {winner1 && (
        <div className="absolute" style={{top: '50%', left: '50%', transform: 'translate(-50%, -50%)', width: '80%'}}>
            <div className="bg-brand-coral text-brand-dark-blue p-4 rounded-lg w-full">
                <Trophy className="mx-auto w-8 h-8 mb-2" />
                <StyledText style={styles.winner1Name} className="truncate">{winner1.participant}</StyledText>
                <StyledText style={styles.winner1Team} className="truncate">{winner1.school}</StyledText>
            </div>
        </div>
      )}
      
      <div className="absolute" style={{top: '80%', left: '50%', transform: 'translate(-50%, -50%)', width: '80%'}}>
        <div className="grid grid-cols-2 gap-4 w-full">
          {otherWinners.map((winner, index) => (
            <div key={winner.id} className="bg-ui-text-light/10 p-3 rounded-lg">
              <StyledText style={styles[`winner${index+2}Name`]} className="truncate">{winner.participant}</StyledText>
              <StyledText style={styles[`winner${index+2}Team`]} className="truncate">{winner.school}</StyledText>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default PosterLayout5;
