import React from 'react';
import { Result, PosterStyles } from '../../../types';

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

const PosterLayout2: React.FC<PosterLayoutProps> = ({ program, winners, styles }) => {
  const sortedWinners = [...winners].sort((a, b) => a.position - b.position);

  function ordinal(num: number) {
    if (num === 1) return "1st";
    if (num === 2) return "2nd";
    if (num === 3) return "3rd";
    return `${num}th`;
  }

  return (
    <div className="w-full h-full bg-ui-surface p-8 font-sans relative overflow-hidden flex flex-col">
      <StyledText style={styles.programName} className="tracking-tight">{program.event}</StyledText>
      <StyledText style={styles.category} className="uppercase tracking-wider">{program.category}</StyledText>
      
      <div className="relative flex-grow mt-24">
        {sortedWinners.map((w, idx) => (
          <div key={w.id} className="flex items-center gap-4 mb-4">
            <span className={`font-bold text-2xl text-brand-coral w-12 text-center`}>{ordinal(w.position)}</span>
            <div>
              <StyledText style={styles[`winner${idx+1}Name`]} className="uppercase tracking-tight">{w.participant}</StyledText>
              <StyledText style={styles[`winner${idx+1}Team`]} className="uppercase tracking-wider">{w.school}</StyledText>
            </div>
          </div>
        ))}
      </div>
      
      <div className="relative mt-auto text-center border-t pt-4">
        <StyledText style={styles.footerTitle} className="leading-tight tracking-tight">MEEM FEST</StyledText>
        <StyledText style={styles.footerSubtitle}>SSF DAAWA SECTOR</StyledText>
      </div>
    </div>
  );
};

export default PosterLayout2;
