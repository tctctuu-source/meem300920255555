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

const PosterLayout1: React.FC<PosterLayoutProps> = ({ program, winners, styles }) => {
  const winner1 = winners.find(w => w.position === 1);
  const winner2 = winners.find(w => w.position === 2);
  const winner3 = winners.find(w => w.position === 3);

  return (
    <div className="w-full h-full bg-gradient-to-br from-brand-dark-blue via-brand-mid-blue to-brand-dark-teal text-ui-text-light font-sans relative overflow-hidden">
      <div className="absolute inset-0 bg-[url('https://i.pinimg.com/736x/37/1d/da/371dda41d9edbc8c24517726b1717a95.jpg')] opacity-10"></div>
      
      <StyledText style={styles.programName} className="uppercase tracking-widest">{program.event}</StyledText>
      <StyledText style={styles.category}>{program.category}</StyledText>

      {winner1 && (
        <>
          <StyledText style={styles.winner1Title}>WINNER</StyledText>
          <StyledText style={styles.winner1Name} className="font-black truncate">{winner1.participant}</StyledText>
          <StyledText style={styles.winner1Team} className="truncate">{winner1.school}</StyledText>
        </>
      )}

      {winner2 && (
        <>
          <StyledText style={styles.winner2Title}>2nd Place</StyledText>
          <StyledText style={styles.winner2Name} className="truncate">{winner2.participant}</StyledText>
        </>
      )}

      {winner3 && (
        <>
          <StyledText style={styles.winner3Title}>3rd Place</StyledText>
          <StyledText style={styles.winner3Name} className="truncate">{winner3.participant}</StyledText>
        </>
      )}
    </div>
  );
};

export default PosterLayout1;
